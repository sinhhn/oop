# Object Oriented in PHP
### Without DI
```
class GoogleMap {
  public function getCoordinateByAddress($address) {
    return getGoogleCoordinate($address);
  }
}

class BingMap {
  public function getCoordinateByAddress($address) {
    return getBingCoordinate($address);
  }
}


class MapService {
  public function getCoordinateByAddress($service, $address) {
    if($service === "GoogleMaps") {
      return new GoogleMap()->getGoogleCoordinate($address);
    } 
    else if( $service === "BingMaps") {
      return new BingMap()->getGoogleCoordinate($address);
    }
    else {
      return "";
    }
  }
}
```

### With DI
```
interface MapServiceInterface {
  public function getCoordinateByAddress($address);
}
class GoogleMap implements MapServiceInterface {
  public function getCoordinateByAddress($address) {
    return getGoogleCoordinate($address);
  }
}

class BingMap implements MapServiceInterface {
  public function getCoordinateByAddress($address) {
    return getBingCoordinate($address);
  }
}


class MapService {
  // DI here 
  protected $service;
  public function __construct($serviceInstance) {
    $this->service = $serviceInstance;
  }
  public function getCoordinateByAddress($address) {
    return $this->service->getCoordinateByAddress($address);
  }
}

class Main {
  public function main() {
    // use google
    $service = new MapService(new GoogleMap());
    echo $service->getCoordinateByAddress($address);
    
    // use Bing
    $service = new MapService(new BingMap());
    echo $service->getCoordinateByAddress($address);
  }
}
```
