# fastapi-pasteo
FastApi user authentication using pasteo using PostgresDB

## Initial setup 
1. Create virtual environment with `python3 -m venv venv`
2. Add Virtual environment path to source in `~/.bashrc` file at the end `source venv/bin/activate` and open new bash session. You will see `(venv)` which means you are in virtual environment 

## Setup postgres
1. Install postgres `sudo apt install postgresql postgresql-contrib`
2. Login as postgres user `sudo -i -u postgres` proceed to postgresql prompt with `psql`
3. Lets create `users` table 
    <code>

        CREATE USER yashram WITH PASSWORD <password>;
        CREATE DATABASE test WITH OWNER yashram; 
        CREATE SCHEMA core AUTHORIZATION yashram; 
        CREATE TABLE core.users ( 
                id serial NOT NULL PRIMARY KEY,
                username varchar unique NOT NULL ,
                email varchar unique NOT NULL,
                password varchar NOT NULL,
                firstname varchar NOT NULL,
                role varchar NOT NULL,
                created_at timestamp default now(),
                updated_at timestamp default now()
            );
    </code>
Note: Make sure you login to new user before creating table 

## Setup fastapi
1. Install required dependencies using `pip install -r requirements.txt`
2. Create /app/main.py with below standard code to start with fastapi
    <code>
        
        from fastapi import FastAPI 

        app = FastAPI()

        @app.get("/")
        async def root():
            return {"message": "Hello World"}
    </code>

3. Run `uvicorn app.main:app --reload` to run fastapi server. Navigate to http://127.0.0.1:8000 to check the server. Hola! you would see below json in your browser which means fast api is running successfully 
    <code>

        {"message":"Hello World"}
    </code>

## Connect database 
1. Update `SQLALCHEMY_DATABASE_URL` in `connectdb.py` with your database credentials 
   <code>

        postgresql://<username>:<password>@<host>:5432/<database_name>
   </code>

## Create ORM model for table 
1. Create orm model in `models.py` for `core.users` table to interact without raw sql commands 

## Create Pydantic models
1. Create `users` related pydantic models in `request_schemas.py` inorder to validate the response in/out through server.

## Create user-signup route
1. Create `create_user` method in `users.py` for signup purpose. We can access the api at `/v1/users/signup/` . Make sure you fill the post request json with `REFERER_SCRECT = 'DEMO_PROJECT'`. This will enable to unauthorised or bot signups as we are using internal authentication to proceed signup. This is one of the process I follow to avoid bot signups. 

## Create user_login route
1. Create `user_login` method in `auth.py` to able to signin. We will get paseto `access_token` in response on successfull atttempt, which is used for session authentication for further operations.
2. Make sure you give some random value to `SECRET_KEY` and give the duration for the active access_token to `ACCESS_TOKEN_EXPIRE_MINUTES` in `oauth.py` file. It would be best practice to store these as env variables.
   
## Get profile details
1. This is sample route `/v1/users/<username>` to access the member only pages which needs authentication.
2. Make sure you use the `access_token` as authorization in header with type as `Bearer` for further member only pages, here accessing the profile details.


For api documentation you can navigate to http://127.0.0.1:8000/docs

## Scope : 
1. You can restrict duplicate active sessions 
2. Method which will able to update the profile details 
3. Implement other member only activity like write,read,modify the posts

# Happy Coding

-- Kasi Yeswanth