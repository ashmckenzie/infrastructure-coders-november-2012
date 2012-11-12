!SLIDE

# Deployments & Operations<br/> at Hooroo #

<br/>
### Ash McKenzie, Developer###
#### November 27 2012 ####


!SLIDE bullets

# Me #

.notes Dev at Hooroo, Sys Admin in past life

!SLIDE

# Hooroo #

[urbandictionary.com/define.php?term=hooroo](http://urbandictionary.com/define.php?term=hooroo)

<cite>An Aussie way of saying "goodbye"</cite>

<cite>
  <table>
    <tr>
      <td>Bruce:</td>
      <td>Crikes it's getting late, mate, I should probably get back.</td>
    </tr>
    <tr>
      <td>Shelia:</td>
      <td>No worries, mate, see you for the shearing competition tomorrow. Hooroo.</td>
    </tr>
    <tr>
      <td>Bruce:</td>
      <td>Hooroo!</td>
    </tr>
</cite>


!SLIDE bullets smbullets

# The company #

* Wholly owned subsidiary of the Qantas Group
* Australian focused travel site
* Provide accomodation booking and inspirational content
* Emphasis on strong visuals and ease of use

![Hooroo](hooroo.png)


!SLIDE bullets smbullets

# The tech #

* EngineYard (AWS)
* Rails 3.2.x
* nginx, unicorn, HAProxy
* PostgreSQL 9.x, Redis

![engine_yard](engine_yard.png)
![rails](rails.png)
![nginx](nginx.png)
![unicorn](unicorn.png)
![postgres](postgres.png)
![redis](redis.png)


!SLIDE bullets smbullets

# Deployments #

* Daily Production deploys (~10AM)
* Leverage Jenkins and Build Pipeline plugin
* Takes approximately eight minutes
* Deployment candidates available once test suite successful


!SLIDE bullets smbullets

# Zero Downtime #

* Customers experience no service loss during deployment
* Achieved using


!SLIDE bullets smbullets

# Asynchronous Airbrake #


!SLIDE bullets smbullets

# RSpec Infrastructure Testing #
