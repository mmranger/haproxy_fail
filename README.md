# haproxy_fail cookbook description

This cookbook simply runs tests using the haproxy cookbook showing potential bug that I ran across.  

## Failure Issue

Issue if the chef run fails after some components of the haproxy cookbook but before the rest then only part of the haproxy.cfg file will be written and this will break the server.  This does not leave the server the way it was found.

## Steps to reproduce:

* kitchen converge config-1-centos-7 > run-logs/before-broke.log
* kitchen verify config-1-centos-7 > run-logs/verify-before-broke.log
* uncomment out test/fixtures/cookbooks/test/recipes/config_1.rb Line 27
* kitchen converge config-1-centos-7 > run-logs/after-broke.log
* kitchen verify config-1-centos-7 > run-logs/verify-after-broke.log

See output in run-logs/
Before-broke converge and verify work. Haproxy config is complete and haproxy is running.

Both after-broke converge and verify fail. Since it failed between haproxy cookbooks resources the result is haproxy is still running but the haproxy.cfg is in an incomplete state.

## Work Around

Simply don't have anything that could potentially fail the chef run inbetween haproxy cookbooks resources
