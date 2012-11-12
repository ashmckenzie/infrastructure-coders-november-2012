!SLIDE

# Operations at Hooroo #

.notes Feel free to ask any questions at any time

<br/>
### Ash McKenzie, Developer###
#### November 13 2012 ####


!SLIDE bullets smbullets

.notes Dev at Hooroo, Sys Admin background, love Ruby

# Me #

* [@ashmckenzie](http://twitter.com/ashmckenzie)
* [http://ashmckenzie.org](http://ashmckenzie.org)


!SLIDE bullets

* Hooroo - What, whom, tech
* Development & Deployment
* Chef build pipeline
* Zero downtime
* Asynchronous Airbrake
* Infrastructure Testing


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
* Backbone brings order to the chaos
* CoffeeScript for developer productivity (and enjoyment!)
* pry allows you to 'freeze' execution and interact
* Zeus - significantly speeds up development & testing

![backbone](backbone.png)
![pry](pry.png)
![coffeescript](coffeescript.png)


!SLIDE bullets smbullets

.notes Jenkins starts prepare job which runs 'fast' unit tests, finishes with close job which will create a deployment candidate

# Deployment #

* Jenkins and Build Pipeline plugin
* Commits to master kick off build and are tested thoroughly
* Successful Staging deploy results in Production deploy candidate
* Daily Production deploys, usually arround 10AM
* Takes approximately eight minutes


!SLIDE bullets smbullets

.notes We have a physical deploy button using an Arduino in the works

# Deployment cont'd #

* Utilise feature toggling of functionality to reduce delays
* Everyone (inc. non techs) deploy to Production
* Deployment is a single click affair in Jenkins

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

.notes We wanted to fallback to the traditional synchronous method in the event of an error, so took advantage of the .async method accepting a block and providing custom functionality

# Asynchronous Airbrake #

![airbrake](airbrake.png)

* Airbrake is great, but can be slow (submitting & their UI)
* New Relic RPM revealed consistent delays in submitting
* When using Ruby 1.9, async mode with [girl_friday](https://github.com/mperham/girl_friday)
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

# Infrastructure Testing #

## Using RSpec ##


!SLIDE bullets smbullets

.notes Using RSpec to test our infrastructure seemed logical, leverage team knowledge

# Infrastructure Testing #

* Use RSpec to test infrastructure
* Common language to all developers
* Lowers barrier to add and update checks
* Integrate with Jenkins
* Drop JSON file containing state
* Open Source once complete


!SLIDE bullets smbullets

# Infrastructure Testing #

## Service definition ##

    @@@ ruby
    # nginx
    #
    shared_examples "an nginx setup" do
      it { host.should have_remote_file '/etc/nginx/nginx.conf' }
      it { host.should listen_on_local_port 81 }
    end

    # postgres
    #
    shared_examples "a postgresql setup" do
      it { host.should have_remote_file '/var/run/postgres.pid' }
      it { host.should listen_on_local_port 5432 }
    end


!SLIDE bullets smbullets

# Infrastructure Testing #

## Environment definition ##

    @@@ ruby
    shared_context "Environment" do
      describe do
        [ :app_master, :app_slaves ].each do |role|
          describe role do
            Nagios::HostManager.hostnames(environment, role).each do |hostname|
              describe hostname do
                it_behaves_like "an nginx setup"
                it_behaves_like "a haproxy setup"
              end
            end
          end
        end
      end
    end


!SLIDE bullets smbullets

# Infrastructure Testing #

## Specific environment definition ##

    @@@ ruby
    describe 'Shrubbery', :environment => :shrubbery do
      include_context "Environment"
    end

!SLIDE bullets smbullets

# Demo #


!SLIDE bullets smbullets

# Thanks :) #

