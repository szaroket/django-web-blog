- [WebBlog](#webblog)
  * [Technologies](#technologies)
  * [ToDo](#todo)
    + [Backend](#backend)
    + [Frontend](#frontend)
  * [Running project locally](#running-project-locally)
    + [Operating system](#operating-system)
    + [Environment](#environment)
    + [Backend](#backend-1)
      - [PostgresSQL](#postgressql)
      - [Python and Django](#python-and-django)
      - [Run backend server](#run-backend-server)
    + [Frontend](#frontend-1)
      - [Run frontend server](#run-frontend-server)
  * [Deployment steps](#deployment-steps)
  * [Implementation](#implementation)
    + [Backend](#backend-2)
    + [Frontend](#frontend-2)

# WebBlog

The purpose of this task is to create an E2E experience of a Blog application.

## Technologies
Here is a list of technologies which should be used for this project:
* Backend:
  * Python (Django + DRF)
* Frontend:
  * Typescrip (React, minimal CSS - MUI5)
* Database:
  * PSQL

Nice to have:
* Firebase

## ToDo 

List of tasks which should be done for this project.

### Backend
- [X] Create a REST API that will allow to:
  - [X] Create a post
  - [X] Edit a post
  - [X] Delete a post
  - [X] List all posts
  - [X] Get single post
- [ ] Add test coverage
- [X] Prepare README:
  - [X] Prepare README
  - [X] Explain deployment steps

Nice to do:
- [ ] Count post views, save it in the database and return in endpoint responses
- [ ] Authorization
- [X] Swagger
- [ ] Permissions:
  - [ ] Admin user should be able to create, edit and delete posts
  - [ ] Standard user should be able to view and list posts

### Frontend
- [X] Blog should contain a list of:
  - [X] All posts 
  - [X] A single post view.
- [X] Fetch data from the API
- [X] Send data to the API
- [X] Prepare README
  - [X] Explain local usage
  - [X] Explain deployment steps

Nice to do:
- [ ] Count post views and display in the post view
- [ ] Responsive design (mobile and tablet friendly) - can use MUI5
- [ ] Authorization
- [X] Axios (HTTP requests)
- [ ] Permissions:
  - [ ] Admin user should be able to create, edit and delete posts
  - [ ] Standard user should be able to view and list posts
  

## Running project locally

In this section, you will find all the steps for preparing an environment to run the WebBlog project locally on your computer.
Please remember that you should run frontend and backend server at the same time to be able to see a fully functioning application. 

### Operating system

Program was created on Windows 10 Home.

### Environment

The whole environment was set up with [conda](https://docs.conda.io/en/latest/) which can be downloaded and installed from [this link](https://docs.conda.io/en/latest/miniconda.html).

### Backend

#### PostgresSQL

Install PostgresSQL from the official PostgreSQL [download section](https://www.postgresql.org/download/).
You can follow [this instruction](https://www.enterprisedb.com/docs/supported-open-source/postgresql/installer/02_installing_postgresql_with_the_graphical_installation_wizard/01_invoking_the_graphical_installer/).

:exclamation: If you will change any of the below names, the application will not work. You will have to change `DATABASES` properties in `backend/mainapp/mainapp/settings.py` file. :exclamation:

After installation, open `pgAdmin` you have to add a new server and create a new empty database:

1. Log in to the PostgreSQL database server using pgAdmin.
2. Go to the `Dashboard` tab. In the `Quick Link` section, click `Add New Server` to add a new connection.
![img_3.png](assets/database1.png)
3. In `General` tab, enter the name of server: PostgreSQL.    
![img_1.png](assets/database2.png)
4. Select the `Connection` tab in the `Create-Server` window. Then, configure the connection as follows:
   1. Host name/addres: localhost
   2. Port: 5432
   3. Maintenance database: `postgres`
   4. Username: postgres
   5. Password: test     
   ![img_2.png](assets/database3.png)
5. Click on `Save` button.
6. In your `PostgreSQL` server, right-click the Databases node and select Create > Database… menu item.
![img.png](assets/database4.png)
7. Enter the name of the database (`blog-database`) and select an owner (`postgres`) in the general tab.
![img.png](assets/database5.png)
8. Click on `Save` button.

#### Python and Django

To run this project, you have to install python and other necessary tools / libraries like: Django, djangorestframework, etc. 

You can do it by following these steps:

1. Clone this repo and go to the main folder (`WebBlog`) in your terminal.
2. Run below command: it will create a conda environment with all dependencies. :warning: Installation can take a few minutes.
    ```commandline
      conda env create -f environment.yaml
    ```
3. Change conda environment with below command:
    ```commandline
    test-env
    ```

#### Run backend server

To run backend server, you have to:
1. Go to `backend/mainapp` in your terminal.
2. Run this command:
    ```commandline
    python manage.py runserver
    ```
3. Go to http://127.0.0.1:8000/docs/ page. You should see Swagger documentation.

![img_4.png](assets/swagger.png)

### Frontend

To run an application you have to install `nodejs` package. If the above steps for the backend have been performed, `nodejs` has been installed while creating a conda environment.

#### Run frontend server

To run frontend server, you have to:
1. Go to `react_app` in your terminal.
2. Run this command:
    ```commandline
    npm start
    ```
3. Go to http://localhost:3000/ page. Here is an example view (depends on the contents in the database).

![img.png](assets/main_page.png)

## Deployment steps

1. As a first step, I prepared an environment. The whole environment was set up with [conda](https://docs.conda.io/en/latest/).
2. Setting up Django project:
   1. Create Django project with `django-admin startproject mainapp` command.
   2. Create a new Django `webblog` app and add it in project settings (i.e. `mainapp/settings.py` file).
3. Install PostgresSQL and create new database by pgAdmin.
4. Connect PostgresSQL database with app by changing `DATABASES` in `mainapp/settings.py`.
5. Migrate schema to a database by `python manage.py migrate` command.
6. Create admin by `python manage.py createsuperuser` command.
7. Create a `Post` model which will store posts from blog. This model was also added to `webblog/adming.py` so admin can create/update/delete posts from the administration site.
8. Create first REST API: getting list of all posts.
9. Create all API methods: create, edit, delete, list all, get single. I decided to have separate API for each request instead of using `RetrieveUpdateDestroyAPIView`. In my opinion, it is more clear that way (which is debatable).
10. Add some basic tests for a `Post` model (1 test doesn't work because of problems with mock time).
11. Start working on frontend. Implemented the main page with all posts view (connected to API by `axios`).
12. Work on `Add post` page and menu. `Post page` was implemented by using `Form`.
13. Work on single post view by using `useLocation`.
14. Add basic CSS and improve the main page view (e.g. cutting post text).
15. Add checks if user provided data to the form - if not, show some warnings. User will be also re-directed to the main page after adding post.
16. Add "read more" button to each post on the main blog page.
17. Create Swagger documentation.

## Implementation

In this section you can find information, what was implemented in the WebBlog project. 

### Backend

Simple API was implemented by using Django REST framework. Five actions in total are supported:
  - Create a post (http://127.0.0.1:8000/post/create/)
  - Edit a post (http://127.0.0.1:8000/post/update/<int:pk> where `<int:pk>` is an id number of post)
  - Delete a post (http://127.0.0.1:8000/post/delete/<int:pk> where `<int:pk>` is an id number of post)
  - List all posts (http://127.0.0.1:8000/posts/)
  - Get single post (http://127.0.0.1:8000/post/<int:pk>/ where `<int:pk>` is an id number of post)

There is also possible to go to `admin` page: http://127.0.0.1:8000/admin/

Additionally, the Swagger documentation was created and is available on http://127.0.0.1:8000/docs/

### Frontend

Simple Blog application was created by using React, TypeScript and CSS. Application operation is limited - currently only 3 activities are supported:
1. The main page with all posts:
![img.png](assets/main_page.png)
2. A single post view:
![img.png](assets/single_post.png)
3. Adding new posts (form):
![img_1.png](assets/add_post_form.png)
