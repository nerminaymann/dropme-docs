# DropMe API Project

## Getting started

### Prerequisites

1- **Install** [git CMD](https://git-scm.com/downloads)
    ```
    Clone Repo : "git clone https://github.com/ahmedsaeed102/dropme.git" command
    Open folder: “cmd dropme” command
    ```
2- **Prepare for project and open it**
    ```
    set up virtual environment : "python -m venv venv" command
    Activate virtual enviroment : 
    "venv/bin/activate"  or   "./venv/scripts/activate.ps1" command
    Install requirements : "pip install -r requirements.txt" command 
    ```

3-	**Install** Django 

4-	**Install** [GeoDjango](https://docs.djangoproject.com/en/4.1/ref/contrib/gis/install/)
    ```
      in installation step : 
        1.Select package GDAL
        2.select spatial extensions from categories
    ```
    ```
      After installation modify windows env in cmd.exe:
        set OSGEO4W_ROOT=C:\OSGeo4W
        set GDAL_DATA=%OSGEO4W_ROOT%\apps\gdal\share\gdal
        set PROJ_LIB=%OSGEO4W_ROOT%\share\proj
        set PATH=%PATH%;%OSGEO4W_ROOT%\bin
        reg ADD "HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\Environment" /v Path /t REG_EXPAND_SZ /f /d "%PATH%"
        reg ADD "HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\Environment" /v GDAL_DATA /t REG_EXPAND_SZ /f /d "%GDAL_DATA%"
        reg ADD "HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\Environment" /v PROJ_LIB /t REG_EXPAND_SZ /f /d "%PROJ_LIB%"
    ```
    
5-	**Install** [PostgreSQL](https://www.postgresql.org/download/)
    ```
      In psql shell :
             “CREATE EXTENSION postgis“ command
             “CERATE EXTENSION postgis_raster” command
    ```
    ```
      Activate it in your project 
      connect to local database and migrate :
          “python manage.py migrate” command
    ```
    
6-  **Put environment variables “.env” file in the root project**

7-	**Edit configurations in project to put environment variables**
    ```
      If you’re using Pycharm  
      open the run configuration selector  
      edit configuration  select the correct file  
      click on Environment variables  change variables  then click ok
    ```

8-	**Run server**
    ```
      python manage.py runserver
    ```
