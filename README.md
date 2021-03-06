# ZohoCRMClient
[![Scrutinizer Quality Score](https://scrutinizer-ci.com/g/christiaan/zohocrmclient/badges/quality-score.png?s=67d109ea323c9024fb43ff1c8a23a5b4c676dbce)](https://scrutinizer-ci.com/g/christiaan/zohocrmclient/)
[![Build Status](https://travis-ci.org/christiaan/zohocrmclient.png)](https://travis-ci.org/christiaan/zohocrmclient)

Provides a clean readable PHP API to the [Zoho Rest API](https://www.zoho.com/crm/help/api/).

## Usage
```php
use Christiaan\ZohoCRMClient\ZohoCRMClient;

$client = new ZohoCRMClient('Leads', 'yourAuthKey');

$records = $client->getRecords()
    ->selectColumns('First Name', 'Last Name', 'Email')
    ->sortBy('Last Name')->sortAsc()
    ->since(date_create('last week'))
    ->request();

echo 'Content: ' . print_r($records, true) . PHP_EOL;

```

## Using custom transport settings to enable logging
```php
$buzzTransport = new BuzzTransport(
    new \Buzz\Browser(new \Buzz\Client\Curl()),
    'https://crm.zoho.com/crm/private/xml/'
);
$buzzTransport->setLogger($logger);

$transport = new XmlDataTransportDecorator(
    new AuthenticationTokenTransportDecorator(
        'yourAuthKey',
        $buzzTransport
    )
);

$client = new ZohoCRMClient('Leads', $transport);
```

## Unimplemented Calls
At the moment only the folowing calls are supported
- getRecords
- insertRecords
- getFields

It is rather easy to add new calls, look at one of the classes in the Request dir for examples.
After the Request class is made it might be nessecary to alter the parsing of the response XML in the XmlDataTransportDecorator class.