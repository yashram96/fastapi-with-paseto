# fastapi-pasteo
fastapi user authentication using pasteo

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
1. Create `users` related pydantic models in `request_scemas.py` inorder to validate the response in/out through server.

## Create user-signup route
1. Create 
