# Game Content Cache Hostnames

## Introduction

This is a list of all hostnames that are required to be redirected for running a game content cache. This list will change frequently so this is designed to be a definitive list.

## Usage

You can use this list one of two ways:

 - Overriding DNS for these hostnames to point to the IP of your cache server.
 - Use them in Squid with WCCP to redirect content to the right cache server.

There is a cache_domains.json file to define CDNs and additional meta deta with the following structure

- cache_domains: Array of cache_domain object
	- name: shortname for the cache domain. Should match `^[0-9A-Za-z]$`
	- description: a longer description to aid others in identifying what this domain does (not all users of this repo will want to enable all caches)
	- notes: implementation specific notes which may be useful for other users
	- mixed_content: true if this domain hosts mixed https and http content (a straight dns injection is unlikely to work in this case). Assumed to be false if undefined
	- domain_files: array of files within the repo assosciated to the cdn. Most cdn's only need one file
	- Example domain entry for origin
```json
{
	"cache_domains": [
		{
			"name": "origin",
			"description": "CDN for origin",
			"notes": "Should be enabled for HTTP traffic only or with a HTTPS proxy else origin client download fails",
			"mixed_content": true,
			"domain_files": ["origin.txt"]
		}
	]
}
```

### Host file format

There is a separate file for each cacheable service. Some notes on formatting:

  - Every line should be a seperate hostname for that service.
  - Only one entry is permitted per line.
  - Wildcards are permitted as per below
  - Lines starting with a # will be treated as a comment.
  - Files must end with an empty newline.
  - File names must match the regular expression `^[0-9A-Za-z].txt`

#### Wildcards

The wildcard format shall be defined as per the below

  - Wildcards should be represented with an asterix.
  - If a wildcard is used, it should be the first character on the line.
  - Wildcards are not treated as matching null, e.g. `*.example.com` will match `a.example.com` but will not match `example.com`
  - Only simple domain wildcards will be accepted eg `*.example.com` not `*ww.example.com`
  
##### Notes for Squid users

If you are using these files within a squid dst_domain acl you will need to reformat the wildcard entries to be compliant with the squid acl format. The following regex should suffice `s/*\./\./`

## Updates

Please fork this repository and submit pull requests if you have any extra hostnames or services to add. We want this list to be definitive and collaborative!

### New CDNs

If the CDN you are adding does not already exist please remember to add an entry within cache_domains.json as well as a new .txt file

## Issues and Feedback

Please raise all issues and feedback on GitHub at [uklans/cache-domains](https://github.com/uklans/cache-domains/issues).

## License

The MIT License (MIT)

Copyright (c) 2017 UK LAN Techs

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
