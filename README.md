# SFS Querier

PHP (meta) package for querying api.stopforumspam.com and providing help analysing/understanding 
results.

## Installation

`composer require webignition/sfs-querier`

## api.stopformumspam.com overview

[api.stopforumspam.com][sfs-usage] can be queried by email address, email hash, ip address
or username. Optional flags can be provided to influence what types of results are returned.

Read the [api.stopforumspam.com usage guide][sfs-usage] first if unfamiliar.

## Usage

- [create client][client-creating-a-client]
- optionally [create a request][client-creating-a-request] 
if not using a single-value convenience method
- [query][client-querying] for results
- understand and analyse results

## Quick Usage Example
```php
use webignition\SfsClient\Client;
use webignition\SfsResultInterfaces\ResultInterface;

$client = new Cient();

// Query against a single email address
$result = $client->queryEmail('user@example.com');

// $result will be NULL if the HTTP request to query api.stopforumspam.com failed for any reason

if ($result instanceof ResultInterface) {
    $result->getType();                 // 'email', 'emailHash', 'ip' or 'username'
    $result->getFrequency();            // int
    $result->getAppears();              // bool
    $result->getValue();                // the email address, email hash, IP address or username
    $result->getLastSeen()              // \DateTime()|null
    $result->getConfidence()            // float|null
    $result->getDelegatedCountryCode(); // string|null
    $result->getCountryCode();          // string|null
    $result->getAsn();                  // int|null
    $result->isBlacklisted();           // bool
    $result->isTorExitNode();           // bool|null
}
```

Read more about [creating requests](/docs/creating-a-request.md) and [querying](/docs/querying.md).

## Understanding and Analysing Results

You probably want to know if a given email address/IP address/username can be trusted 
to perform an operation within your application.

```php
use webignition\SfsResultAnalyser\Analyser;
use webignition\SfsResultInterfaces\ResultInterface;

// We're assuming that $result is a ResultInterface object.

$analyser = new Analyser();

// Does a given result indicate that the entity is not to be trusted?
$analyser->isUntrustworthy($result));
// returns true or false

// Is a given result's entity trustworthy?
// Return a float between 0 (do not trust) and 1 (probably can be trusted)
$trustworthiness = $analyser->calculateTrustworthiness($result);
```

## See Also
Use [webignition/sfs-querier](https://github.com/webignition/sfs-querier) for a package that
contains [webignition/sfs-result-analyser](https://github.com/webignition/sfs-result-analyser),
[webignition/sfs-client](https://github.com/webignition/sfs-client) and provides detailed
usage instructions.

[sfs-usage]: https://www.stopforumspam.com/usage
[client-creating-a-client]: https://github.com/webignition/sfs-client/blob/master/docs/creating-a-client.md
[client-creating-a-request]: https://github.com/webignition/sfs-client/blob/master/docs/creating-a-request.md
[client-querying]: https://github.com/webignition/sfs-client/blob/master/docs/querying.md
