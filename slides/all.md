!SLIDE

# Deployments & Operations<br/> at Hooroo #

.notes Feel free to ask any questions at any time

<br/>
### Ash McKenzie, Developer###
#### November 13 2012 ####


!SLIDE bullets smbullets

.notes Dev at Hooroo, Sys Admin background, love Ruby

# Me #

* [@ashmckenzie](http://twitter.com/ashmckenzie)
* [http://ashmckenzie.org](http://ashmckenzie.org)


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
* Australian focused travel site, for travellers
* Provides accomodation booking and inspirational content
* Emphasis on strong visuals and ease of use

![Hooroo](hooroo.png)


!SLIDE bullets smbullets

# The tech #

* EngineYard (AWS)
* Ruby 1.9.3, Rails 3.2.x
* nginx, Unicorn, HAProxy
* PostgreSQL 9.x, Redis

![engine_yard](engine_yard.png)
![ruby](ruby.png)
![rails](rails.png)
![nginx](nginx.png)
![unicorn](unicorn.png)
![postgres](postgres.png)
![redis](redis.png)


!SLIDE bullets smbullets

# Deployments #

* Leverage Jenkins and Build Pipeline plugin
* Deployment candidates available once test suite successful
* Daily Production deploys (~10AM)
* Takes approximately eight minutes
* Utilise feature toggling of functionality to reduce delays


!SLIDE bullets smbullets

# Chef build pipeline #



!SLIDE bullets smbullets

# Zero Downtime #

* Customers experience no service loss during deployment
* Unicorn reload (USR2) and careful DB migrations

![maintenance](maintenance.png)

!SLIDE bullets smbullets

# Asynchronous Airbrake #


!SLIDE bullets smbullets

.notes Every Thursday we spend the afternoon trying out new tech, experimenting with new ideas / concepts.

# 10% time sneak peek #

## RSpec Infrastructure Testing ##

* Use RSpec to test infrastructure
* Common language to all developers
* Lowers barrier to add and update checks
* Plan on Open Sourcing once complete


!SLIDE bullets smbullets

# RSpec Infrastructure<br/> Testing demo #


!SLIDE bullets smbullets

# Thanks :) #


