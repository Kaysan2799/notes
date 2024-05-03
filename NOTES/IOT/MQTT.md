>[!TIP]  ## MQTT installation process
>### Install: 
>	sudo apt-install mosquitto-client
>### Start
>	sudo systemctl start mosquitto
>### status
>	sudo systemctl status mosquitto
>### Create file for password:
>	sudo touch /etc/mosquitto/passwd
>### Set Sub password:
>	sudo mosquitto_password -c /etc/mosquitto/passwd subscriber_user password
>### Set Pub password:
>	sudo mosquitto_passwd -c /etc/mosquitto/passwd publisher_user 
>### Open config file:
>	sudo nano /etc/mosquitto/mosquitto.conf 
>	// Add this in it:
>		include_dir /etc/mosquitto/conf.d
>		password_file /etc/mosquitto/passwd
>		allow_anonymous true
>		listner 1887 0.0.0.0
>### Restart service:
>	systemctl restart mosquitto
>### Start subscriber:
>	mosquitto_sub -h localhost -t "topic/test" -u "user" -P "passwd" 
>### Send msg:
>	mosquitto_pub -h localhost -t test/topic -m "Abdul Malik" 



