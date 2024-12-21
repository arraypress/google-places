# Google Places API for WordPress

A comprehensive PHP library for integrating with the Google Places API in WordPress, providing easy-to-use methods for geocoding, place details, search, and autocomplete functionality. Features WordPress integration, robust caching, and `WP_Error` support.

## Features

- 🔍 **Comprehensive Search**: Support for text, nearby, and autocomplete searches
- 📍 **Place Details**: Detailed place information including address, contact details, and reviews
- 🌍 **Geocoding**: Convert addresses to coordinates and vice versa
- 🕒 **Opening Hours**: Formatted business hours with current status
- ⭐ **Reviews & Ratings**: Access to user reviews and rating information
- 📸 **Photo Integration**: Easy access to place photos with customizable dimensions
- 💰 **Price Levels**: Support for price range information
- 🚗 **Distance Matrix**: Calculate distances between locations
- 🛡️ **Type Safety**: Full type hinting and strict types
- ⚡ **Built-in Caching**: Efficient caching system for API responses
- 🌐 **Multiple Languages**: Support for international queries and responses
- 🎯 **Parameter Management**: Comprehensive parameter handling through traits
- ✨ **Easy Implementation**: Simple, chainable API methods
- 🔄 **Method Chaining**: Fluent interface for setting options
- ⚙️ **Validation**: Built-in parameter validation for all setters

## Requirements

- PHP 7.4 or later
- WordPress 5.0 or later
- Google Places API key

## Installation

Install via Composer:

```bash
composer require arraypress/google-places
```

## Basic Usage

```php
use ArrayPress\Google\Places\Client;

// Initialize client with your API key
$client = new Client( 'your-google-api-key', true, DAY_IN_SECONDS );

// Get place details
$place = $client->get_place_details( 'place-id' );
$address = $place->get_formatted_address();
$coordinates = $place->get_coordinates();

// Search for places
$results = $client
    ->set_search_type( 'restaurant')
    ->set_language( 'en')
    ->find_places( 'pizza in Seattle' );

// Nearby search
$nearby = $client
    ->set_open_now( true )
    ->nearby_search( 47.6062, -122.3321, 1000 );

// Autocomplete
$predictions = $client
    ->set_autocomplete_types( [ 'establishment' ] )
    ->get_autocomplete_predictions( 'Space Needle' );
```

## Configuration Methods

### Setting Options

```php
// Search parameters
$client->set_search_type( 'restaurant' );    // Set place type
$client->set_search_keyword( 'pizza' );      // Set search keyword
$client->set_language( 'en' );               // Set language
$client->set_min_price( 1 );                 // Set minimum price level (0-4)
$client->set_max_price( 3 );                 // Set maximum price level (0-4)
$client->set_open_now( true );               // Filter for open places
$client->set_rank_by( 'distance' );          // Set ranking method

// Autocomplete parameters
$client->set_autocomplete_types( [ 'establishment' ] );
$client->set_autocomplete_location( 47.6062, -122.3321, 5000 );
$client->set_strict_bounds( true );

// Photo parameters
$client->set_photo_max_width( 800 );
$client->set_photo_max_height( 600 );

// Cache settings
$client->set_cache_enabled( true );
$client->set_cache_expiration( HOUR_IN_SECONDS );
```

### Getting Options

```php
// Search configuration
$type = $client->get_search_type();        // Current search type
$keyword = $client->get_search_keyword();   // Current search keyword
$language = $client->get_language();        // Current language
$min_price = $client->get_min_price();     // Current minimum price
$open_now = $client->get_open_now();       // Current open now filter

// Autocomplete settings
$types = $client->get_autocomplete_types();
$location = $client->get_autocomplete_location();
$bounds = $client->get_strict_bounds();

// Photo settings
$max_width = $client->get_photo_max_width();
$max_height = $client->get_photo_max_height();

// Cache settings
$cache_enabled = $client->is_cache_enabled();
$cache_expiration = $client->get_cache_expiration();
```

## Extended Examples

### Working with Place Details

```php
$place = $client->get_place_details( 'place-id' );

// Get basic information
$name = $place->get_formatted_address();
$coordinates = $place->get_coordinates();
$phone = $place->get_formatted_phone_number();
$website = $place->get_website();

// Get structured address
$address = $place->get_structured_address();
echo $address['street_number'] . ' ' . $address['street_name'];
echo $address['city'] . ', ' . $address['state'];

// Check opening hours
$hours = $place->get_formatted_opening_hours();
$is_open = $place->is_open_now();
echo $place->get_opening_hours_html();

// Get reviews and ratings
$rating = $place->get_rating();
$reviews = $place->get_reviews();
$total_ratings = $place->get_user_ratings_total();

// Get photos
$photos = $place->get_photos();
foreach ( $photos as $photo ) {
    $photo_url = $client->get_place_photo_url( $photo['photo_reference'] );
}
```

### Search Operations

```php
// Text search with filters
$client
    ->set_search_type( 'restaurant')
    ->set_min_price( 2 )
    ->set_max_price( 4 )
    ->set_open_now( true );

$results = $client->text_search( 'fine dining Seattle' );

// Nearby search with ranking
$client
    ->set_search_type( 'cafe')
    ->set_rank_by( 'distance' );

$nearby = $client->nearby_search( 47.6062, -122.3321, 1000 );

// Autocomplete with location bias
$client
    ->set_autocomplete_types( [ 'establishment' ] )
    ->set_autocomplete_location( 47.6062, -122.3321, 5000 )
    ->set_strict_bounds( true );

$predictions = $client->get_autocomplete_predictions( 'Space' );
```

### Geocoding

```php
// Forward geocoding
$response = $client->geocode( '1600 Amphitheatre Parkway, Mountain View, CA' );
$location = $response->get_coordinates();

// Structured address geocoding
$address = [
    'street' => '1600 Amphitheatre Parkway',
    'city' => 'Mountain View',
    'state' => 'CA'
];
$response = $client->geocode( $address );
```

## API Methods

### Main Methods
* `get_place_details( $place_id, $fields = [] )`: Get detailed place information
* `find_places( $query )`: Search for places by text query
* `nearby_search( $lat, $lng, $radius )`: Search for nearby places
* `text_search( $query )`: Perform a text search
* `geocode( $address )`: Geocode an address
* `get_autocomplete_predictions( $input )`: Get place predictions
* `get_place_photo_url( $photo_reference )`: Get photo URL
* `clear_cache( $identifier = null )`: Clear cached data

### Response Methods
* `get_formatted_address()`: Get formatted address
* `get_coordinates()`: Get location coordinates
* `get_structured_address()`: Get detailed address components
* `get_opening_hours()`: Get opening hours
* `is_open_now()`: Check if place is currently open
* `get_rating()`: Get place rating
* `get_reviews()`: Get place reviews
* `get_photos()`: Get place photos
* `get_website()`: Get place website
* `get_phone_number()`: Get contact number
* `get_price_level()`: Get price level
* `get_business_status()`: Get business status

## Use Cases

* **Business Directory**: Create comprehensive business listings
* **Store Locator**: Build store/location finder systems
* **Autocomplete Forms**: Enhance address input forms
* **Business Details**: Display rich business information
* **Location Validation**: Verify and standardize addresses
* **Review Display**: Show business reviews and ratings
* **Opening Hours**: Display business hours and status
* **Photo Galleries**: Show place photos
* **Price Information**: Display price level indicators
* **Business Search**: Implement location-based search
* **Address Verification**: Validate and format addresses
* **Geographic Data**: Work with location coordinates

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is licensed under the GPL-2.0-or-later License.

## Support

- [Documentation](https://github.com/arraypress/google-places)
- [Issue Tracker](https://github.com/arraypress/google-places/issues)