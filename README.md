Introduction
============

Clone of [TS3 PHP Framework 1.1.23](https://www.planetteamspeak.com/) providing the library as composer project including namespaces and follow the PSR-4 Standard.


Usage
=====

Install via composer:

    "require": {
        "hardwiregaming/teamspeak-php-framework": "~1.1"
    },

Example:

```php
namespace Foo;
use TeamSpeak3\TeamSpeak3;

class TeamSpeak3Adapater
{
    private $ts;
    public __construct()
    {
        $this->ts3 = TeamSpeak3::factory("serverquery://username:password@127.0.0.1:10011/?server_port=9987");
    }

    public writeMessage($message)
    {
        $this->ts3->message($message);
    }
}
```


For more information visit the [official documentation][1].

[1]: https://ts3-php.readthedocs.org
