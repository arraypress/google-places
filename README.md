# ArrayPress Google Places

A powerful PHP library for integrating with the Google Places API in WordPress. This library provides a clean, intuitive interface for handling place data, geocoding, and business information, with specialized WordPress integration.

## Features

- 🗺️ **Comprehensive Place Data**: Complete access to Google Places information
- 📍 **Geocoding**: Convert addresses to coordinates and vice versa
- 💼 **Business Details**: Access hours, ratings, reviews, and contact info
- 🔍 **Location Search**: Find and filter nearby places
- ⚡ **WordPress Integration**: Native transient caching and WP_Error support
- 🛡️ **Type Safety**: Full type hinting and strict types
- 🔄 **Utility Functions**: Tools for formatting and validating place data
- 📐 **Distance Calculations**: Built-in distance and radius calculations
- 🖼️ **Static Maps**: Generate static map URLs for places

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
use ArrayPress\GooglePlaces\Client;

// Initialize client with your API key
$client = new Client('your-google-api-key');

// Geocode an address
$result = $client->geocode('1600 Amphitheatre Parkway, Mountain View, CA');
if (!is_wp_error($result)) {
    $coordinates = $result->get_coordinates();
    echo "Latitude: {$coordinates['latitude']}\n";
    echo "Longitude: {$coordinates['longitude']}\n";
}
```

## Feature Examples

### Place Details

Get comprehensive information about a specific place:

```php
// Fetch place details
$details = $client->get_place_details($place_id);

if (!is_wp_error($details)) {
    $name = $details->get_formatted_address();
    $phone = $details->get_phone_number();
    $website = $details->get_website();
    $rating = $details->get_rating();
    $is_open = $details->is_open_now() ? 'Open' : 'Closed';
}
```

### Nearby Search

Find places near a specific location:

```php
// Search for restaurants within 1km
$nearby = $client->nearby_search(37.4220, -122.0841, 1000, [
    'type' => 'restaurant',
    'keyword' => 'italian'
]);

if (!is_wp_error($nearby)) {
    foreach ($nearby->get_results() as $place) {
        echo $place['name'] . "\n";
    }
}
```

### Utility Functions

Helper methods for common place-related tasks:

```php
use ArrayPress\GooglePlaces\Places;

// Calculate distance between points
$distance = Places::calculate_distance(
    37.4220, -122.0841,  // Point A
    37.4219, -122.0782   // Point B
);

// Generate a static map URL
$map_url = Places::get_static_map_url(
    'your-api-key',
    37.4220,
    -122.0841
);

// Format opening hours
$hours = Places::format_opening_hours($place_details->get_opening_hours());

// Format business status
$status = Places::format_business_status($place_details->get_business_status());
```

## API Methods

### Client Methods
- `geocode($address)`: Convert address to coordinates
- `get_place_details($place_id, $fields = [])`: Get place details
- `find_places($query, $params = [])`: Search places by text
- `nearby_search($lat, $lng, $radius, $params = [])`: Find nearby places

### Response Methods
- `get_coordinates()`: Get latitude/longitude
- `get_formatted_address()`: Get full address
- `get_place_id()`: Get Google Place ID
- `get_business_status()`: Get operational status
- `get_opening_hours()`: Get business hours
- `get_phone_number()`: Get contact number
- `get_website()`: Get business website
- And many more...

### Utility Methods
- `calculate_distance()`: Calculate point distance
- `format_opening_hours()`: Format business hours
- `is_valid_place_id()`: Validate place IDs
- `format_price_level()`: Format price levels
- `get_static_map_url()`: Generate map URLs
- `is_within_radius()`: Check radius inclusion

## Use Cases

- Store Locators: Build store/location finder systems
- Business Directories: Create business listing websites
- Address Validation: Verify and standardize addresses
- Location-Based Services: Develop proximity-based features
- Maps Integration: Add maps to your WordPress site
- Business Information: Display business details and hours
- Geocoding Services: Convert addresses to coordinates

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is licensed under the GPL-2.0-or-later License.

## Support

- [Documentation](https://github.com/arraypress/google-places)
- [Issue Tracker](https://github.com/arraypress/google-places/issues)