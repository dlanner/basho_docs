---
title: Riak CS Release Notes
project: riakcs
version: 1.2.0+
document: cookbook
index: true
audience: intermediate
keywords: [developer]
---

{{#1.4.3+}}

## Riak CS 1.4.3

[Riak CS 1.4.3 Release Notes](https://github.com/basho/riak_cs/blob/1.4.3/RELEASE-NOTES.org)

#### Bugs Fixed

* Fix bug that reverted manifests in the scheduled_delete state to the pending_delete or active state.
* Don't count already deleted manifests as overwritten
* Don't delete current object version on overwrite with incorrect md5

#### Additions

* Improve performance of manifest pruning
* Optionally use paginated 2i for the GC daemon. This is to help prevent timeouts when collecting data that can be garbage collected.
* Improve handling of Riak disconnects on block fetches
* Update to lager 2.0.1
* Optionally prune manifests based on count, in addition to time
* Allow multiple access archiver processes to run concurrently

{{/1.4.3+}}

## Riak CS 1.4.2

#### Bugs Fixed
* Fix issue with Enterprise build on Debian Linux distributions.
* Fix source tarball build.
* Fix access statistics bug that caused all accesses to be treated as
  errors.
* Make logging in bucket listing map phase function lager version
  agnostic to avoid issues when using versions of Riak older than 1.4.
* Handle undefined `props` field in manifests to fix issue accessing
  objects written with a version of Riak CS older than 1.3.0.

#### Additions
* Add option to delay initial GC sweep on a node using the
  `initial_gc_delay` configuration option.
* Append random suffix to GC bucket keys to avoid hot keys and improve
  performance during periods of frequent deletion.
* Add `default_proxy_cluster_id` option to provide a way to specify a
  default cluster id to be used when the cluster id is undefined. This is
  to facilitate migration from the OSS version to the
  Enterprise version.

## Riak CS 1.4.1

#### Bugs Fixed
* Fix list objects crash when more than the first 1001 keys are in
  the pending delete state.
* Fix crash in garbage collection daemon.
* Fix packaging bug by updating node_package dependency.

## Riak CS 1.4.0

#### Additions
* Add preliminary support for the Swift API and Keystone authentication.
* Improve performance of object listing when using Riak 1.4.0 or greater.
* Add ability to edit user account name and email address.
* Add support for v3 multi-data-center replication.
* Add configurable Riak connection timeouts.
* Add syslog support via Lager.
* Only contact one vnode for immutable block requests.

#### Bugs Fixed
* Remove unnecessary keys in GC bucket.
* Fix query-string authentication for multi-part uploads.
* Fix Storage Class for multi-part uploaded objects.
* Fix etags for multi-part uploads.
* Support reformat indexes in the Riak CS multi-backend.
* Fix unbounded memory-growth on GET requests with a slow connection.
* Reduce access-archiver memory use.
* Fix 500 on object ACL HEAD request.
* Fix semantics for concurrent upload and delete of the same key with a
  multi-part upload.
* Verify content-md5 header if supplied.
* Handle transient Riak connection failures.

## Riak CS 1.3.1

#### Bugs Fixed
* Fix bug in handling of active object manifests in the case of
  overwrite or delete that could lead to old object versions being
  resurrected.
* Fix improper capitalization of user metadata header names.
* Fix issue where the S3 rewrite module omits any query parameters
  that are not S3 subresources. Also correct handling of query
  parameters so that parameter values are not URL decoded twice. This
  primarily affects pre-signed URLs because the access key and request
  signature are included as query parameters.
* Fix for issue with init script stop.

## Riak CS 1.3.0

#### Additions
* Support for multipart file uploads. Parts must be in the range of
  5MB-5GB.
* Support for bucket policies using a restricted set of principals and
  conditions.
* Support for returning bytes ranges of a file using the `Range` header.
* Administrative commands may be segregated onto a separate interface.
* Authentication for administrative commands may be disabled.
* Performance and stability improvements for listing the contents of
  buckets.
* Support for the prefix, delimiter, and marker options when listing
  the contents of a bucket.
* Support for using Webmachine's access logging features in
  conjunction with the Riak CS internal access logging mechanism.
* Moved all administrative resources under `/riak-cs`.
* Riak CS now supports packaging for FreeBSD, SmartOS, and Solaris.

#### Bugs Fixed
* Fix handling of cases where buckets have siblings. Previously this
  resulted in 500 errors returned to the client.
* Reduce likelihood of sibling creation when creating a bucket.
* Return a 404 instead of a 403 when accessing a deleted object.
* Unquote URLs to accommodate clients that URL encode `/` characters
  in URLs.
* Deny anonymous service-level requests to avoid unnecessary error
  messages trying to list the buckets owned by an undefined user.

## Riak CS 1.2.2

#### Additions
* Full support for MDC replication

#### Bugs Fixed
* Fix problem where objects with utf-8 unicode key cannot be listed
  nor fetched.
* Speed up bucket_empty check and fix process leak. This bug was
  originally found when a user was having trouble with `s3cmd
  rb :s3//foo --recursive`. The operation first tries to delete the
  (potentially large) bucket, which triggers our bucket empty
  check. If the bucket has more than 32k items, we run out of
  processes unless +P is set higher (because of the leak).

## Riak CS 1.2.1

#### Additions
* Add reduce phase for listing bucket contents to provide backpressure
  when executing the MapReduce job.
* Use prereduce during storage calculations.
* Return 403 instead of 404 when a user attempts to list contents of
  nonexistent bucket.

#### Bugs Fixed
* Return 403 instead of 404 when a user attempts to list contents of
  nonexistent bucket.
* Do not do bucket list for HEAD or ?versioning or ?location request.

## Riak CS 1.2.0

#### Additions
* Add preliminary support for MDC replication
* Quickcheck test to exercise the erlcloud library against Riak CS
* Basic support for riak_test integration

#### Bugs Fixed
* Do not expose stack traces to users on 500 errors
* Fix issue with sibling creation on user record updates
* Fix crash in terminate state when fsm state is not fully populated
* Script fixes and updates in response to node_package updates

## Riak CS 1.1.0

#### Additions
* Update user creation to accept a JSON or XML document for user
  creation instead of URL encoded text string.
* Configuration option to allow anonymous users to create accounts. In
  the default mode, only the administrator is allowed to create
  accounts.
* Ping resource for health checks.
* Support for user-specified metadata headers.
* User accounts may be disabled by the administrator.
* A new key_secret can be issued for a user by the administrator.
* Administrator can now list all system users and optionally filter by
  enabled or disabled account status.
* Garbage collection for deleted and overwritten objects.
* Separate connection pool for object listings with a default of 5
  connections.
* Improved performance for listing all objects in a bucket.
* Statistics collection and querying.
* DTrace probing.

#### Bugs Fixed
* Check for timeout when checking out a connection from poolboy.
* PUT object now returns 200 instead of 204.
* Fixes for Dialyzer errors and warnings.
* Return readable error message with 500 errors instead of large webmachine backtraces.

## Riak CS 1.0.2

#### Additions
* Support query parameter authentication as specified in [http://docs.amazonwebservices.com/AmazonS3/latest/dev/RESTAuthentication.html](Signing and Authenticating REST Requests).

## Riak CS 1.0.1

#### Bugs Fixed
* Default content-type is not passed into function to handle PUT
  request body
* Requests hang when a node in the Riak cluster is unavailable
* Correct inappropriate use of `riak_cs_utils:get_user` by
  `riak_moss_acl_utils:get_owner_data`

## Riak CS 1.0.0

#### Additions
* Subsystem for calculating user access and storage usage
* Fixed-size connection pool of Riak connections
* Use a single Riak connection per request to avoid deadlock conditions
* Object ACLs
* Management for multiple versions of a file manifest
* Configurable block size and max content length
* Support specifying non-default ACL at bucket creation time

#### Bugs Fixed
* Fix PUTs for zero-byte files
* Fix fsm initialization race conditions
* Canonicalize the entire path if there is no host header, but there are
  tokens
* Fix process and socket leaks in get fsm

## Riak CS 0.1.2

#### Bugs Fixed
* Return 403 instead of 503 for invalid anonymous or signed requests.
* Properly clean up processes and connections on object requests.

## Riak CS 0.1.1

#### Bugs Fixed
* HEAD requests always result in a `403 Forbidden`.
* `s3cmd info` on a bucket object results in an error due to missing
  ACL document.
* Incorrect atom specified in `riak_moss_wm_utils:parse_auth_header`.
* Bad match condition used in `riak_moss_acl:has_permission/2`.

## Riak CS 0.1.0

#### Additions
* Bucket-level access control lists
* User records have been modified so that an system-wide unique email
  address is required to create a user.
* User creation requests are serialized through `stanchion` to be
  certain the email address is unique.
* Bucket creation and deletion requests are serialized through
  `stanchion` to ensure bucket names are unique in the system.
* The `stanchion` serialization service is now required to be installed
  and running for the system to be fully operational.
* The concept of an administrative user has been added to the system. The credentials of the
  administrative user must be added to the app.config files for `moss` and `stanchion`.
* User credentials are now created using a url-safe base64 encoding module.

#### Bugs Fixed
* `s3cmd info` fails due to missing `last-modified` key in return document.
* `s3cmd get` of 0 byte file fails.
* Bucket creation fails with status code `415` using the AWS Java SDK.

#### Known Issues
* Object-level access control lists have not yet been implemented.

## Riak CS 0.0.3

#### Additions
* Support for the s3cmd subcommands sync, du, and rb
* Return valid size and checksum for each object when listing bucket objects.
* Changes so that a bucket may be deleted if it is empty.
* Changes so a subdirectory path can be specified when storing or retrieving files.
* Make buckets private by default
* Support the prefix query parameter
* Enhance process dependencies for improved failure handling

#### Bugs Fixed
* URL decode keys on put so they are represented correctly. This
  eliminates confusion when objects with spaces in their names are
  listed and when attempting to access them.
* Properly handle zero-byte files
* Reap all processes during file puts

#### Known Issues
* Buckets are marked as /private/ by default, but globally-unique
    bucket names are not enforced. This means that two users may
    create the same bucket and this could result in unauthorized
    access and unintentional overwriting of files. This will be
    addressed in a future release by ensuring that bucket names are
    unique across the system.
