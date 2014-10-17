StravaPHP - WIP
=========
TLDR; Strava V3 REST PHP client with OAuth authentication. [Get started](#Get started)

The Strava V3 API is a publicly available interface allowing developers access 
to the rich Strava dataset. The interface is stable and currently used by the 
Strava mobile applications. However, changes are occasionally made to improve 
performance and enhance features. See the [changelog](http://strava.github.io/api/v3/changelog/) for more details.

In this GitHub repository you can find the PHP implementation of the 
Strava V3 API. The current version of StravaPHP combines the V3 API 
with a proper OAuth authentication.

## Methods
### Athlete
```php
$strava->getAthlete($id = null);
$strava->getAthleteClubs();
$strava->getAthleteActivities($before = null, $after = null, $page = null, $per_page = null);
$strava->getAthleteFriends($id = null, $page = null, $per_page = null);
$strava->getAthleteFollowers($id = null, $page = null, $per_page = null);
$strava->getAthleteBothFollowing($id, $page = null, $per_page = null);
$strava->getAthleteKom($id, $page = null, $per_page = null);
$strava->getAthleteStarredSegments($id = null, $page = null, $per_page = null);
$strava->updateAthlete($city, $state, $country, $sex, $weight);
```

### Activity
```php
$strava->getActivity($id, $include_all_efforts = null);
$strava->getActivityComments($id, $markdown = null, $page = null, $per_page = null);
$strava->getActivityKudos($id, $page = null, $per_page = null);
$strava->getActivityPhotos($id);
$strava->getActivityZones($id);
$strava->getActivityLaps($id);
$strava->getActivityUploadStatus($id);
$strava->createActivity($name, $type, $start_date_local, $elapsed_time, $description = null, $distance = null);
$strava->uploadActivity($file, $activity_type = null, $name = null, $description = null, $private = null, $trainer = null, $data_type = null, $external_id = null);
$strava->updateActivity($name = null, $type = null, $private = false, $commute = false, $trainer = false, $gear_id = null, $description = null);
$strava->deleteActivity($id);
```

### Gear
```php
$strava->getGear($id);
```

### Club
```php
$strava->getClub($id);
$strava->getClubMembers($id, $page = null, $per_page  = null);
$strava->getClubActivities($id, $page = null, $per_page  = null);
```

### Segment
```php
$strava->getSegment($id);
$strava->getSegmentLeaderboard($id, $gender = null, $age_group = null, $weight_class = null, $following = null, $club_id = null, $date_range = null, $page = null, $per_page = null);
$strava->getSegmentExplorer($bounds, $activity_type = 'riding', $min_cat = null, $max_cat = null);
$strava->getSegmentEffort($id, $athlete_id = null, $start_date_local = null, $end_date_local = null, $page = null, $per_page = null);
```

### Streams
```php
$strava->getStreamsActivity($id, $types, $resolution = 'all', $series_type = 'distance');
$strava->getStreamsEffort($id, $types, $resolution = 'all', $series_type = 'distance');
$strava->getStreamsSegment($id, $types, $resolution = 'all', $series_type = 'distance');
```

## Get started
### Get your API key
All calls to the Strava API require an access token defining the athlete and 
application making the call. Any registered Strava user can obtain an access 
token by first creating an application at [strava.com/developers](http://www.strava.com/developers)

### Install 
Use composer to install this StravaPHP package.

TODO: update composer usage

### Use it! 
```php
<?php 
include 'vendor/autoload.php';

use Strava\API\OAuth as OAuth;
use Strava\API\V3\Client as StravaClient;
use Strava\API\V3\ServiceREST as ServiceREST;

try {
    $OAuth = new OAuth(array(
        'clientId'     => 'CLIENT ID',
        'clientSecret' => 'SECRET HERE',
        'redirectUri'  => 'URL ENDPOINT'
    ));
    
    if (!isset($_GET['code'])) {
        print '<a href="'.$OAuth->getAuthorizationUrl().'">connect</a>';
    } else {
        $token = $OAuth->getAccessToken('authorization_code', array(
            'code' => $_GET['code']
        ));
        
        $strava = new StravaClient(new ServiceREST($token));
        $activities = $strava->getAthleteActivities(null, null, null, 2);
        print_r($activities);
    }
    
} catch(Exception $e) {
    print $e->getMessage();
}
```

## Thanks guys!
- [Strava API](http://strava.github.io/api/)
- [thephpleague/oauth2-client](https://github.com/thephpleague/oauth2-client/)
- [respect/Validation](https://github.com/respect/Validation)
- [educoder/pest] (https://github.com/educoder/pest)
