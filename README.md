# OpenID Connect

This extension lets you authenticate Frontend users against an OpenID Connect server. It is preconfigured to work with
the [WSO2 Identity Server](http://wso2.com/identity-and-access-management) from the Swiss Alpine Club but may be used
with your own identity server as well.

If you are a Swiss Alpine Club section, be sure to get in touch with Bern in order to get your dedicated private key and
secret.


## Configuring

### Mapping Frontend User Groups

- Create your groups within TYPO3
- Use the additional pattern to relate it to roles within OpenID Connect
- Local TYPO3 groups (not related to some role) will be kept upon authenticating
- Default TYPO3 group(s) as configured in Extension Manager will always be added


## Logging

This extension makes use of the Logging system introduced in TYPO3 CMS 6.0. It is far more flexible than the old one
writing to the "sys_log" table. Technical details may be found in the
[TYPO3 Core API](https://docs.typo3.org/typo3cms/CoreApiReference/ApiOverview/Logging/Index.html#logging).

As an administrator, what you should know is that the TYPO3 Logger forwards log records to "Writers", which persist the
log record.

By default, with a vanilla TYPO3 installation, messages are written to the default log file
(`typo3temp/logs/typo3_*.log`).


### Dedicated log file for OpenID Connect

If you want to redirect every logging information from this extension to `typo3temp/logs/oidc.log` and send log
entries with level "WARNING" or above to the system log, you may add following configuration to
`typo3conf/AdditionalConfiguration.php`:

```
$GLOBALS['TYPO3_CONF_VARS']['LOG']['Causal']['Oidc']['writerConfiguration'] = [
    \TYPO3\CMS\Core\Log\LogLevel::DEBUG => [
        \TYPO3\CMS\Core\Log\Writer\FileWriter::class => [
            'logFile' => 'typo3temp/logs/oidc.log'
        ],
    ],

    // Configuration for WARNING severity, including all
    // levels with higher severity (ERROR, CRITICAL, EMERGENCY)
    \TYPO3\CMS\Core\Log\LogLevel::WARNING => [
        \TYPO3\CMS\Core\Log\Writer\SyslogWriter::class => [],
    ],
];
```

**Hint:** Be sure to read
[Configuration of the Logging system](https://docs.typo3.org/typo3cms/CoreApiReference/ApiOverview/Logging/Configuration/Index.html#logging-configuration)
to fine-tune your configuration on any production website.
