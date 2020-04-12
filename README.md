# Passport ID validator

* Validation service for Russian password using FMS data

# Dependencies

* Nginx
* RabbitMQ
* php
* php-fpm
* Redis
* phpredis
* MySQL
* php-mysql
* cron
* https://xn--b1ab2a0a.xn--b1aew.xn--p1ai/upload/expired-passports/list_of_expired_passports.csv.bz2

# Expected Input
* Passport serial number and passport id

```json
{
  "serial": 1243,
  "id": 1234567
}
```


# Output

```json
{
  "success": true,
  "is_valid": true
}
```
```json
{
  "success": true,
  "is_valid": false
}
```
```json
{
  "success": false,
  "errors": [
    {
      "message": "error message"
    }
  ]
}
```

# Infrastructure
![Image description](https://i.imgur.com/gxFJULg.jpg)

* Nginx round robin-load balancer in front of service
* RabbitMq cluster for queue api requests. Receives requests from nginx balancer
* Nginx + PHP-fpm + php app instances
* Redist cluster as primary data source for data requests
* MySql in master-slave mode. Slave replicas for fall-back reading in case of Redis not-found results or failures 
* Single app instance for download CSV data from source and update data in redis and mysql both
* Cron job to run csv app for update

# Issues

- [ ] Set up Nginx in round-robin balancer mode
- [ ] Set up rabbitMq cluster
- [ ] Set up redis cluster
- [ ] PHP api
    - [ ] Create api handler
    - [ ] Handle and validate request data
    - [ ] Read data from redis, fallback to mysql slave in case of failure. Write result in redis if needed.
    - [ ] Return response
 - [ ] Console app for download CSV from source and populate DB data
    - [ ] Resumable CSV data file download from source
    - [ ] Parse and update redis and mysql master 
- [ ] Set up cron job
