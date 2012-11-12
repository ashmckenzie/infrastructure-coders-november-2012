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
      <td>Shaz:</td>
      <td>No worries mate, see you for the shearing competition tomorrow. Hooroo.</td>
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

.notes Using Zeus lately, speeds up the use of Rails console, faster testing response, etc

# Development goodies #

* JavaScript heavy app
* Backbone to bring order to the chaos
* CoffeeScript for developer productivity (and enjoyment!)
* pry allows you to 'freeze' execution and interact
* Zeus which significantly speeds up development & testing

![backbone](backbone.png)
![pry](pry.png)
![coffeescript](coffeescript.png)


!SLIDE bullets smbullets

# Deployments #

* Jenkins and Build Pipeline plugin
* Checkins to master kick off build and are tested thoroughly
* Deployment candidates available once test suite successful
* Daily Production deploys (~10AM)
* Takes approximately eight minutes


!SLIDE bullets smbullets

# Deployments cont'd #

* Utilise feature toggling of functionality to reduce delays
* Everyone (inc. non techs) deploy to Production
* Deployment is a single click affair

![deploy](deploy.png)


!SLIDE bullets smbullets

.notes Jenkins detects chef related changes, kicks off a dedicated Chef pipeline build

# Chef build pipeline #

* Chef recipes live within main codebase
* Chef changes detected and procesed by Jenkins
* Aim to mirror code deployment process
* Reduces environment inconsistencies


!SLIDE bullets smbullets

.notes The USR2 signal causes Unicorn to create a new master process, whilst keeping the previous master running serving the old app.  If the new Unicorn master process starts up successfully then the old master process is killed.  In the event the new Unicorn master process cannot start up properly, the old Unicorn master process continues serving clients.

# Zero Downtime #

* Customers experience no service loss during deployment
* Unicorn reload (USR2) and careful DB migrations
* Great [RailsCast](http://railscasts.com/episodes/373-zero-downtime-deployment) recently covered all the gotchas

![maintenance](maintenance.png)


!SLIDE bullets smbullets

# Asynchronous Airbrake #

* Airbrake is great, but can be slow (submitting & their UI)
* New Relic RPM revealed consistent delays in submitting
* Updated to use async Airbrake submission via Resque
* Fallback to synchronous method if Resque job fails


!SLIDE[tpl=code] bullets smbullets

# Asynchronous Airbrake #

### config/initialisers/airbrake.rb ###

    @@@ ruby
    Airbrake.configure do |config|
      config.api_key = 12345678

      config.async do |notice|
        begin
          Resque.enqueue(AirbrakeDeliveryWorker, notice.to_xml)
        rescue
          # job submission failed, so go the slower route.
          Airbrake.sender.send_to_airbrake(notice)
        end
      end
    end


!SLIDE bullets smbullets

.notes Every Thursday we spend the afternoon trying out new tech, experimenting with new ideas / concepts.

# 10% time sneak peek #

## RSpec Infrastructure Testing ##

* Use RSpec to test infrastructure
* Common language to all developers
* Lowers barrier to add and update checks
* Potentially integrate with Jenkins
* Open Source once complete


!SLIDE bullets smbullets

# RSpec Infrastructure Testing #

    @@@ ruby
    # nginx
    #
    shared_examples "an nginx setup" do
      it { @host.should have_remote_file '/etc/nginx/servers/hotels.conf' }
      it { @host.should listen_on_local_port 81 }
    end

    # postgres
    #
    shared_examples "a postgresql setup" do
      it { @host.should have_remote_file '/db/postgresql/9.1/data/postmaster.pid', :sudo => true }
    end


!SLIDE bullets smbullets

# RSpec Infrastructure<br/> Testing demo #


!SLIDE bullets smbullets

# Thanks :) #

