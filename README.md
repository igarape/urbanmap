 

Urban Map is a geospatial open-source product that allows users to input and see location-based information. It is developed using Python and Vue.JS, and includes three modules: `core-api`, `user-interface`, and `utils`.

  
## Installation


First clone the repo using Git.

```
git clone https://github.com/igarape/urbanmap
```

To run Urban Map, you will need to have Docker installed. Once Docker is installed, you can run the following command in the root of the directory.


```

docker-compose up

```


This will start the application and all its modules, with the database set up locally. To change the database URI location go to the docker-compose file and change it up in the variable **SQLALCHEMY_DATABASE_URI**. See the reference below for more details.

Once the application is running, you can access the Urban Radar user interface by visiting `http://localhost:3000` in your web browser.

It's also possible to run Urban Map on any docker friendly cloud product like Cloud Run, App Engine Flex, Kubernetes or EKS by deploying the docker image on each folder inside the repo.

## Modules

### `core-api`

The `core-api` module includes the backend APIs for Urban Map. This module is responsible for handling all API responses. It is built using Python 3.10 and uses a PostgreSQL database to store and retrieve data. When using docker compose it's not necessary to have python installed locally.

It's possible to connect to the database locally to make changes (creating users, roles, layers, adding data) but it's not necessary since that's avaible through the interface. 

One common workflow that might be useful is to add charts for data layers. This can be done by accessing the database via a database explorer tool like **Datagrip** or **Pgadmin**. 

The initial connection info is displayed below.

```
postgres:postgres@localhost:5432/postgres

user: postgres
password: postgres
host: localhost
database: postgres

```

#### Adding charts

When connected to the database it's possible to add charts to a data layer. Charts are added by going into the Chart table and copying a Chart template. Some chart templates are available and it's only necessary to change from where the chart retrieves information.

RAW data is stored at the datalake schema, and the chart query must use the datalake schema to reference information.

#### Useful tables

When importing data for the first time, it will be located at the **datalake** schema. The processed data will be available at the **web** schema.

The web schema owns processed data. 

##### Chart tables

Chart tables are Chart, ChartType and ChartTypeTabLayer. The last one makes a chart available at a specific layer. ChartType creates a new chart type template and Chart makes a chart for that instance.

##### Feature

Feature tables are Feature, FeatureCollection, Field, Properties, FieldConfig, FieldConfigLayer, FieldConfigOptions.

Feature is a representation of a event and information about that event is stored at the Field table. Properties represents the characteristics of a Feature (colors, outlines, sizing, labels). The initial script to input data located at the **utils** module imports automatically data into Feature and Properties, creating the first events that can be consumed through the interface module.

##### Layer

Layer tables are Layer, Section, Tenant. Layer is a representation of grouped information, a Feature is always on a Layer otherwise it's not visible on the interface. A Layer is grouped by Sections. The first Layer and Section is created automatically by the **utils** module when importing the first events.

##### User

User tables are User, Role, RoleScope, Scope, UserRoleTenant. User is always a representation of a User and does contain user info. Scopes are micro actions the user can do inside UrbanMap and they are grouped by Roles. An User always have a Role, and that Role is attached to the user by the UserRoleTenant table. This way we can say which user can access which resource in the interface. This can be created via the interface in the **management** section.


### `user-interface`

The `user-interface` module includes the frontend interface for Urban Map. This module is responsible for providing users with an intuitive and user-friendly interface for accessing location-based information. It is built using React and communicates with the `core-api` module to retrieve and display data.


### First login

Once the ```docker compose up``` command is run, the interface will be available at ```localhost:3000``` and it's possible to first login using the combination below:
```
user admin@urbanmap.com 
password 123456
```

### The user interface

Once logged in Urban Map will automatically be redirected to the default city (Brazil-Pernambuco) and show the geojson matching the city. Also there are layers pre defined which can be used for product demos. E.g. Through the calendar select first of january of 2021 and press the eye icon in emergency calls.

![[Pasted image 20230524160544.png]]

The sidebar also includes the management tab (which can be used to add new users) and the administration tab (which can be used to add new layers, sections and filters).


### `utils`


The `utils` module includes various ETL scripts and other utilities that are used by the `core-api` and `user-interface` modules.

The utils module contains specific documentation on how to use the scripts listed to add new features from a events file, to add a new city geojson and to add filters from a event file.

## Contributions
  

We welcome contributions to Urban Radar! If you would like to contribute, please fork the repository and submit a pull request.

  

## License

  

Urban Radar is licensed under the [MIT License](https://opensource.org/licenses/MIT).
