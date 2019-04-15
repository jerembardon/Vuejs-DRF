#Simple Vue/DRF Exemple

> Warning: This version is not suitable for production, it's just a test of Vue Js with DRF


## Install the project

### Backend part
#### Step 1: env & requirements

Setting up virtualenv and requirements, you need to setting up your env with python 3.

```
cd ./vue-django

virtualenv env

.\env\Scripts\activate

cd backend/

pip install -r requirements.txt
```

#### Step 2: create migration & setting up user


Create super user with these login infos:
⋅⋅* username: test
⋅⋅* email: test@test.fr
⋅⋅* password: test$2012

<Still in backend folder>

```
python manage.py makemigrations
python manage.py migrate

python manage.py createsuperuser
python manage.py runserver
```

### Frontend part
#### Step 1: Npm install

```
cd frontend/
npm i --save 
npm run dev
```

