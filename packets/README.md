In this directory there are a few pcap files you can use to write unit
tests for your DNS server.

They're provided in 2 formats (which both contain the same data): a `.pcap`
that you can open it with Wireshark and a `.txt` hex-encoded file. The idea
is that you'll use the `.txt` file to write tests and use Wireshark to check
out what the correct way to parse the packet is.

### the files

* `dns-request-a-record.pcap`: Generated with `dig @localhost -p 6363 example.com`. Request for an A record for `example.com`. This is the kind of request your DNS server will receive. Generated with 
* `authoritative.pcap`: Generated with `dig @198.41.0.4 example.com`. Request + response to an authoritative root nameserver to get the NS records for `example.com`. You'll need to make requests like this to an authoritative nameserver and parse the response.

If you want, you can also generate your own pcap files with other cases you
want to test with `sudo tcpdump -i any -p 53 -w output.pcap`.

### example of how to use the `.txt` file in a test

In Python 3:

```
def test_whatever():
    with open('dns-request-a-record.txt') as f:
        packet = bytes.fromhex(f.read().strip())
    # test your code that decodes the packet here
```
