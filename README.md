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

        CREATE DATABASE test;
        CREATE USER yashram WITH PASSWORD <password>;
        CREATE DATABASE test WITH OWNER yashram; 
        CREATE SCHEMA core AUTHORIZATION yashram; 
        CREATE TABLE core.users ( 
                id serial NOT NULL PRIMARY KEY,
                name varchar NOT NULL ,
                created_at timestamp default now()
            ); 
    

## Setup fastapi
1. Install required dependencies using `pip install -r requirements.txt`
2. Create /app/main.py with below standard code to start with fastapi
    <code>
        
        from fastapi import FastAPI 

        app = FastAPI()

        @app.get("/")
        async def root():
            return {"message": "Hello World"}

3. Run `uvicorn app.main:app --reload` to run fastapi server. Navigate to http://127.0.0.1:8000 to check the server. Hola! you would see below json in your browser which means fast api is running successfully 
    <code>

        {"message":"Hello World"}