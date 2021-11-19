# TotalCRM MicrosoftGraphBundle

## Installation

### Add MicrosoftGraphBundle to your project

The recommended way to install the bundle is through Composer.

```bash
$ composer require 'totalcrm/microsoft_graph_bundle'
```

## Configuration 

You have to configure your api:
``` yml
microsoft_graph:
    client_id: "%env(MS_GRAPH_CLIENT_ID)%" 
    client_secret: "%env(MS_GRAPH_CLIENT_SECRET)%"
    redirect_uri: "name of your redirect route"
    prefer_time_zone: "%env(TIMEZONE_UTC)%" # your prefered timezone default UTC
    version: "1.0" #version of API GRAPH: #1.0 or beta,  default 1.0
    stateless: true # if false, the state will stored in session
    scopes:  # for more details https://developer.microsoft.com/en-us/graph/docs/authorization/permission_scopes
        - openid
        - offline_access
            #- ...
            
```

# Get  token from Office 365 | API Graph
``` php
    // Get client service 
    $client= $this->get('microsoft_graph.client');
    try{
        /*
         if you have a refresh token then  the token will refresh 
         */
        $client->getNewToken();

    }catch(\Exception $ex){
        // else 
        $client->redirect(); // redirect to office 365 authentication page
    }


```


# Example get events from outlook calendar

``` php
// Get calendar service 
    $calendar= $this->get('microsoft_graph.calendar');
            
//Get a collection of Microsoft\Graph\Model\Event
    $startTime = new DateTime("first day of this month");
    $endTime = new DateTime("first day of next month");
    
    $events = $calendar->getEvents($startTime,$endTime);

//Get a  Microsoft\Graph\Model\Event
    $id='...'
    $event= $calendar->getEvent($id);
     
```


 # Create an event
   ``` php    
            
//  create Microsoft\Graph\Model\Event and set properties
    $newEvent= new Microsoft\Graph\Model\Event();              
    $start= $calendar->getDateTimeTimeZone(new \DateTime('Now next minute'));
    $end= $calendar->getDateTimeTimeZone(new \DateTime('Now next hour'));
    
    $newEvent->setSubject('Controller Test Token');
    $newEvent->setStart($start);
    $newEvent->setEnd( $end);     

    $event= $calendar->addEvent( $newEvent);
     
```

            dump($event);
 # Update an event
``` php
    $id='...'
    $updateEvent= new Microsoft\Graph\Model\Event(); 
    $updateEvent->setId($id);
    $updateEvent->setSubject("I Forgot The Eggs!");
    $event= $calendar->updateEvent( $updateEvent);

``` 

 # Delete an event
``` php
    $id='...'
    $response= $calendar->deleteEvent( $id);
    dump($response->getStatus()==204?"Event deleted":$response);

```


### TODOS
> Abstract Entities
> Documentation