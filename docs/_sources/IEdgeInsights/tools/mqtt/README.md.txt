# MQTT Temperature Sensor
Simple Python temperature sensor simulator.

## Usage
> **NOTE:** This assumes you have already installed and configured Docker.

1. Building and bringing up the tool
   - Run the broker (Use `docker ps` to see if the broker has started successfully as the container starts in detached mode)
    ```sh
    $ ./broker.sh <port>
    ```
    **NOTE:** To run the broker for EII TimeSeries Analytics usecase:
    ```sh
    $ ./broker.sh 1883
    ```

   - Start the mqtt-publisher
   ```sh
   $ cd publisher
   $ set -a
   $ source ../../../build/.env
   $ set +a
   $ docker-compose build
   $ docker-compose up -d
   ```


**Note** By default the tool publishes temperature data. If the user wants to publish other data, he/she needs to modify the command option in "ia_mqtt_publisher" service in  [docker-compose.yml](./publisher/docker-compose.yml) accordingly and recreate the container using `docker-compose up -d` command from publisher directory.

   * To publish temperature data to default topic, command option by default is set to:
   ```sh
    ["--temperature", "10:30"]
   ```

   * To publish temperature and humidity data together, change the command option to:
   ```sh
   ["--temperature", "10:30", "--humidity", "10:30", "--topic_humd", "temperature/simulated/0"]
   ```

   * To publish multiple sensor data(temperature, pressure, humidity) to default topic(temperature/simulated/0, pressure/simulated/0, humidity/simulated/0),change the command option to:
   ```sh
	["--temperature", "10:30", "--pressure", "10:30", "--humidity", "10:30"] 
   ```

   * To publish differnt topic instead of default topic, change the command option to:
   ```sh
	["--temperature", "10:30", "--pressure", "10:30", "--humidity", "10:30", "--topic_temp", <temperature topic>, "--topic_pres", <pressure topic>, "--topic_humd", <humidity topic>]
   ```

  It is possible to publish more than one sensor data into single topic, in that case, same topic name needs to be given for that sensor data. 

   * For publishing data from csv row by row, change the command option to:
     ```sh
     ["--csv", "demo_datafile.csv", "--sampling_rate", "10", "--subsample", "1"]
     ```

   * To publish JSON files (to test random forest UDF), change the command option to:
     ```sh
     ["--topic", "test/rfc_data", "--json", "./json_files/*.json", "--streams", "1"]
     ```

2. If one wish to see the messages going over MQTT, run the
   subscriber with the following command:
   ```sh
   $ ./subscriber.sh <port>
   ```
   Example:
   If Broker runs at port 1883, to run subscriber use following command:

   ```sh
   $ ./subscriber.sh 1883
   ```

