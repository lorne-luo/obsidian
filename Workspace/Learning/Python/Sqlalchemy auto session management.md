
Sqlalchemy ORM docs suggestion that application should [manage the session lifecycle externally  to the function deal with specific data](https://docs.sqlalchemy.org/en/20/orm/session_basics.html#when-do-i-construct-a-session-when-do-i-commit-it-and-when-do-i-close-it). 

Like example below, session will be send into the data manipulate func as args:
```python
class ThingOne:
    def go(self, session):
	    ...
	
    def go2(self, session):
	    ...
	
    def run(self, session):
        with sessionmaker(bind=engine) with session:
		    self.go(session)
		    self.go2(session)
		    session.commit()
```



### How can we refine

To make the session management smarter, I created `auto_session_func` decorator and `auto_session_ctx` context manager which:
- if `session` arg provided, developer have manually control the session generation and commit
- if `session` not provided, new session will be created automatically and commit when exiting the function

So a complicated session operation like this 
```python
def run():
	with sessionmaker(bind=engine) with session:
		session.begin()
		model1_update...
		session.commit()
	
	...
	
	with sessionmaker(bind=engine) with session:
		session.begin()
		model2_update...
		session.commit()
```

can be turned to
```python
@auto_session_func
def model1_update1(*, session=None):
	"""wrap atomic model update as func"""
	model1_update...

def model1_update2(*, session=None):
	"""wrap atomic model update as func"""
	model1_update...

@auto_session_func
def model2_update(*, session=None):
	model2_update...
	
def run1():
	# auto mode, atomically run in separate session
	model1_update1()
	model1_update2()
	...
	model2_update2()
	
def run2()
	# manual mode, control everything by your self
	# the function not as atomic anymore
	with sessionmaker(bind=engine) with session:
		session.begin()
		model1_update1(session=session)
		model1_update2(session=session)
		model2_update(session=session)
		session.commit()
		
	# the atomic funcs model1_update1, modexl1_update2 are flexible in all senarios
```


## Benefit

- make the func super flexible, can be reused in atomic way or non atomic way
- reduce lots of line to create session and commit
- session can be easily externally managed for every methods / code block


or use `auto_session_ctx`
```python

def run(*, session=None):
	with auto_session_ctx(session) with session:
		model1_update1(session=session)
		model1_update2(session=session)
	
	...
	
	with auto_session_ctx(session) with session:
		model2_update(session=session)

# in high level invoker

# if not care the session, for example some select
run()# no session provided

# manually control the session
with sessionmaker(bind=engine) with session:
	run(session)
	session.commit()
```


## Implementation
```python
def auto_session_func(func):  
    """  
    auto session management decorator, apply to methods with a session argument  
    - if session argument is None,        this decorator will create a new session(with transaction begin)        then pass to the origin method        when origin method finished the session will commit automatically    - if session argument is not None,        do nothing, call origin method directly    """  
    actual_func = func  
    if isinstance(func, (staticmethod, classmethod)):  
        actual_func = getattr(func, "__func__")  
  
    @wraps(actual_func)  
    def wrapper(*args, **kwargs):  
        session = kwargs.get("session", None)  
        is_commit = kwargs.get("is_commit", True)  
        if session is None:  
            with get_pg_session() as new_session:  
                new_session.begin()  
                kwargs["session"] = new_session  
                result = actual_func(*args, **kwargs)  
                if is_commit:  
                    new_session.commit()  
                return result  
        else:  
            return actual_func(*args, **kwargs)  
  
    return wrapper
    

```