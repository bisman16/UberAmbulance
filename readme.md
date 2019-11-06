# uberAmbulance

Uber for Ambulance (uberAmbulance) is a spinoff from the popular ride hailing apps such as Uber, Lyft and Via. The goal of uberAmbulance is to facilitate the on-demand nature of ambulance needs with the flexible ability of connecting patients to nearby ambulances with much cheaper rates than is currently typically charged by ambulance companies. UberAmbulance is a private organization that would assist in providing a public utility with the ultimate desire in providing the best possible transportation ambulatory care, in the shortest amount of time, at the cheapest price possible to the patient. 

The system, Uber for Ambulance (uberAmbulance), will assist patients in the facilitation of ambulatory care via a mobile application and make it easier for patients to access emergency transportation in their area at the right time for the best price possible



## mongoDB crash-course

1. Go to [mongoDB community](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x) and follow instructions for install (assuming [homebrew](https://brew.sh/) is your system package-manager). 

	* `brew tap mongodb/brew`

	* `brew install mongodb-community@4.0`

	* `brew services start mongodb-community@4.0`

2. Fire-up mongo by typing `mongo`

3. check dbs by running `show dbs`

4. If no database in use, create one- e.g. `use uberAmbulance`

5. Check collections by running `show collections`.

6. Assuming you have a json file ready for import, run `mongoimport --db uberAmbulance --collection ambulanceData < ambulances.json`

	* In this example, the seed-file is contained within the root-folder of this repo: 'ambulances.json'.

7. Check success by running steps 2-5 above.

8. After confirming steps 2-5, run this command to return the assets ingested into the mongoDB: `db.ambulanceData.find().pretty()`; this command will display all entities and attributes contained within the json-file ingested into mongo-database.

9. To fetch information about a particular ambulance whose userId is 01, type: `db.ambulanceData.find({userId: "01"}).pretty()`.

10. Create a geospatial index- or 2-d sphere index on the location field featuring a type-field within it: `db.ambulanceData.createIndex({"location": "2dsphere"})`

11. This command will locate ambulances that are located within 2 kilometers from latitude 38.902001.  `db.ambulanceData.find({    location: {        $near: {            $geometry: {                type: "Point",                coordinates: [-77.050637, 38.902001]            },            $maxDistance: 2000        }    }}).pretty()`



## npm crash-course

1. After downloading the node-dependencies (package.json) or cloning this repo, run `sudo npm install -g npm-check-updates` for node package-manager updater.

2. Then run `ncu -u`

3. Next run `npm update`

4. Run `npm install`

5. If experiencing any issues related to user-authorization or npm-permissions, run `sudo chown -R $USER:$(id -gn $USER) /Users/username/.config`.
	* e.g. `sudo chown -R $USER:$(id -gn $USER) /Users/alexanderjsingleton/.config` 

6. To launch the web-app server, run `node app js`- basic unit tests:

	* FAIL? http://localhost:8000/ambulances?lat=12.9718915&&lng=77.64115449999997

	* PASS: http://localhost:8000/citizen.html

	* PASS: http://localhost:8000/ambulance.html

	* PASS: http://localhost:8000/citizen.html?userId=YOURNAME

	* PASS: Check web-socket integration by running: http://localhost:8000/ambulance.html?userId=02 and/or http://localhost:8000/citizen.html?userId=02

8. Ambulance views successfully render data-sets ingested within mongodb seed-file, 'ambulances.json'- to verify, start the app by running `node app.js` then check the following views in sequence:

	* http://localhost:8000/ambulance.html?userId=02

	* http://localhost:8000/citizen.html?userId=1 OR http://localhost:8000/citizen.html?userId=alexanderjsingleton

	* After opening-up those views above in separate windows, click the "Request for help" button in either one of the citizen.html views above (please note that you must first start the app, ***then open an ambulance view before requesting for help in the citzen.html view***).

	* After initiating a request from the citizen.html view, all of the ambulance-views should display "You have a new request!" in addition to the citizen.html id details.

	* Click on the "Help Citizen" button within the ambulance view and the database will record/acknowledge that help is on the way.




