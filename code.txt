
CREATE TABLE IF NOT EXISTS Towns(
	town_id smallint PRIMARY KEY UNIQUE NOT NULL,
	town_name varchar(255) NOT NULL
) ENGINE InnoDB;


CREATE TABLE IF NOT EXISTS Stations(
	town smallint NOT NULL,
	station_id smallint UNIQUE NOT NULL,
	station_name varchar(255) NOT NULL,
	PRIMARY KEY (station_id)
)ENGINE InnoDB;
	


CREATE TABLE IF NOT EXISTS Routes(
	route_id smallint   NOT NULL UNIQUE,
	wheel_type varchar(255) NOT NULL ,
	seat_number smallint NOT NULL,
	PRIMARY KEY (route_id)
)ENGINE InnoDB;


CREATE TABLE IF NOT EXISTS Bus_route(
	route smallint NOT NULL ,
	start_time DATETIME NOT NULL,
	end_time DATETIME NOT NULL,
	distance float NULL,
	from_station smallint NOT NULL,
	to_station smallint NOT NULL
)ENGINE InnoDB;



LOAD DATA LOCAL INFILE 'Bus_routes.txt' INTO TABLE Bus_route;
LOAD DATA LOCAL INFILE 'Routes.txt' INTO TABLE Routes;
LOAD DATA LOCAL INFILE 'Stations.txt' INTO TABLE Stations;
LOAD DATA LOCAL INFILE 'Towns.txt' INTO TABLE Towns;


ALTER TABLE Stations
ADD FOREIGN KEY (town)
REFERENCES Towns(town_id) ON UPDATE CASCADE;
 
ALTER TABLE Bus_route
ADD FOREIGN KEY (route)
REFERENCES Routes(route_id) ON UPDATE CASCADE;

ALTER TABLE Bus_route
ADD FOREIGN KEY (from_station) REFERENCES Stations (station_id),
ADD FOREIGN KEY (to_station) REFERENCES Stations (station_id);

DROP TRIGGER IF EXISTS Delete_route;
DELIMITER $$
CREATE TRIGGER Delete_route BEFORE DELETE  ON Routes
FOR EACH ROW 
 BEGIN
   DELETE FROM Bus_route WHERE route = OLD.route_id;

 END $$
DELIMITER ;











