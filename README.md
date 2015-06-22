Background
==========
Folsom is an Erlang-based application that can collect realtime metrics and publish them. More details can be found at: 

`https://github.com/boundary/folsom <https://github.com/boundary/folsom>`_

This CloudI service allows other CloudI services to define new Folsom metrics, update those metrics, and get the current metric values.

Usage
=====

Service Configuration
---------------------
An example configuration is shown below:
        [{type, internal},
         {prefix, "/folsom/"},
         {module, cloudi_service_folsom},
         {args, []},
         {dest_refresh, immediate_random},
         {timeout_init, 60000}, 
         {timeout_async, 60000}, 
         {timeout_sync, 60000}, 
         {dest_list_deny, undefined}, 
         {dest_list_allow, undefined}, 
         {count_process, 1}, 
         {max_r, 5}, 
         {max_t, 900}, 
         {options, [{reload, true}, {queue_limit, 500}]}
        ]

Service Patterns
----------------
This service subscribes to the following request patterns:
<service prefix>/metrics
<service prefix>/metric_value?metric_name
<service prefix>/new_counter?metric_name
<service prefix>/notify?update_value where update value is of  the form {metric_name, value} or 
	{metric_name, {inc, value}} or {metric_name, {dec, value}}


Examples using HTTP requests
----------------------------
The following examples use the Curl command to execute an HTTP request

# define the HTTP endpoint
export FOLSOM_HTTP=http://localhost:6467/folsom

# Get list of metrics
curl $FOLSOM_HTTP/metrics

# Add a new counter metric
curl $FOLSOM_HTTP/new_counter?test_requests

# Get the current value of the counter
curl $FOLSOM_HTTP/metric_value?test_requests

# Update the counter value
# Note the use of the curl encoding feature to properly format the request
curl -G $FOLSOM_HTTP/notify --data-urlencode "{test_requests, {inc,1}}"

