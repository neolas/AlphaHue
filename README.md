# AlphaHue; Phillips Hue PHP SDK
## Synopsis
This is a quick-to-implement library that connects your PHP project to the Phillips Hue lighting system. When I started writing an app for the Hue I found a need for an SDK that was complete and uncomplex with decent documentation. AlphaHue is an attempt to fill that need and speed up the process of getting started.

AlphaHue uses the [PhpRestClient](https://github.com/innesian/PhpRestClient) to interact with the Hue APIs.
## Setup
### Installation with Composer.
Clone the repository.
```
$ git clone https://github.com/innesian/AlphaHue.git
```
Install Composer in your project using cURL (command below) or [download the composer.phar directly](http://getcomposer.org/composer.phar).
```
$ curl -sS http://getcomposer.org/installer | php
```
Let Composer install the project dependencies:
```
$ php composer.phar install
```
Once installed, include the autoloader in your script.
```php
<?php
include_once 'vendor/autoload.php'; // Path to autoload.php file.

// There are instructions on how to obtain Hostname and Username below.
$bridge_hostname = '192.168.1.1';
$bridge_username = 'xxxxxxxxxxx';

$hue = new \AlphaHue\AlphaHue($bridge_hostname, $bridge_username);
```
### (or) add AlphaHue as a dependency to your Hue project using Composer.
Create a *composer.json* file in your project and add `adam-innes/alpha-hue` as a required dependency.
```
{
    "require": {
        "adam-innes/alpha-hue": "1.0.*"
    }
}
```
### Discovering Bridge Hostname and creating a Username
[Phillips has a how-to on getting the internal IP address and creating a username](http://www.developers.meethue.com/documentation/getting-started)
* Make sure the Bridge is connected to the network and working. Check that your smartphone can connect.
* Get the internal IP address from your router -or- use the [broker service](http://www.meethue.com/api/nupnp).
* Follow the directions on obtaining a username from the [documentation](http://www.developers.meethue.com/documentation/getting-started).

#### Lights API
Turn lights on/off.
```php
// Get all light IDs.
$light_ids = $hue->getLightIds();

// Turning lights on by ID.
foreach ($light_ids as $light_id) {
    $hue->togglePower($light_id, 'on');
}

// Turning lights off by ID.
foreach ($light_ids as $light_id) {
    $hue->togglePower($light_id, 'off');
}
```
Get the power status of a light by ID.
```php
$powered_on = $hue->getLightOnStatus($light_id); // True if on, false if off.
```
Delete a light from the Bridge.
```php
$hue->deleteLight($light_id);
```
Attempt to match a light to a color by hex (not all colors can be created by the light).
```php
# Set the light to Red.
$hue->setLightToHex($light_id, '#ff0000');
```
Attempt to match a light to a color by RGB value (not all colors can be created by the light).
```php
# Set the light to Blue.
$rgb = array(
  'r' => 0,
  'g' => 0,
  'b' => 255
);
$hue->setLightToRGB($light_id, $rgb);
```
Get or set the state of a light.
```php
# Get the state of light 1
$state = $hue->getLightState(1);

# Sets the light state to a Red, max brightness.
$red = $hue->getXYPointFromHex('#ff0000');
$lightAttributes = array(
  'on'  => true,
  'bri' => 254, // max brightness is 254
  'xy'  => $red
);
$hue->setLightState(1, $lightAttributes);
```
#### Groups API
Get group IDs associated to the Bridge.
```php
$group_ids = $hue->getGroups();
```
Create a group.
```php
# Create a group titled 'New Group'
$response = $hue->createGroup('New Group');
```
Set the attributes of a group.
```php
# Change group 1 name and set lights 1 and 2 to be the group members.
$attributes = array(
  'name'  => 'New Group Name',
  'lights' => array(1, 2),
);
$hue->setGroupAttributes(1, $attributes);
```
Delete a group.
```php
# Delete group 1.
$hue->deleteGroup(1);
```
Set the state of a group.
```php
$red = $hue->getXYPointFromHex('#ff0000');
$groupAttributes = array(
  'on'  => true,
  'bri' => 254, // max brightness is 254
  'xy'  => $red
);
$hue->setGroupState(1, $groupAttributes);
```
#### Rules API
Get rules.
```php
# Get list of rules stored in the bridge.
$rules = $hue->getRules();

# Get rule with a rule ID of 1.
$rule = $hue->getRule(1);
```
Delete a rule.
```php
# Delete a rule with ID 1.
$hue->deleteRule(1);
```
#### Sensors API
```php
# Get an array of all sensor information.
$sensors = $hue->getSensors();
```
#### Schedule API
Get all schedules.
```php
# Get schedules.
$schedules = $hue->getSchedules();
```
#### Bridge Configuration Information
```php
$hue->config['name']; // Bridge Name.
$hue->config['apiversion']; // Bridge API Version.
$hue->config['ipaddress']; // Bridge IP Address
```
#### 
