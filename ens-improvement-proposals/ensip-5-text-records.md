---
description: A standard for storage of text records in ENS (formerly EIP-634).
---

# ENSIP-5: Text Records

| **Author**    | Richard Moore (@ricmoo) |
| ------------- | ----------------------- |
| **Status**    | Final                   |
| **Submitted** | 2017-05-17              |

### Abstract

This ENSIP defines a resolver profile for ENS that permits the lookup of arbitrary key-value text data. This allows ENS name holders to associate e-mail addresses, URLs and other informational data with a ENS name.

### Motivation

There is often a desire for human-readable metadata to be associated with otherwise machine-driven data; used for debugging, maintenance, reporting and general information.

In this ENSIP we define a simple resolver profile for ENS that permits ENS names to associate arbitrary key-value text.

### Specification

#### Resolver Profile

A new resolver interface is defined, consisting of the following method:

```solidity
interface IERC634 {
  /// @notice Returns the text data associated with a key for an ENS name
  /// @param node A nodehash for an ENS name
  /// @param key A key to lookup text data for
  /// @return The text data
  function text(bytes32 node, string key) view returns (string text);
}
```

The EIP-165 interface ID of this interface is `0x59d1d43c`.

The `text` data may be any arbitrary UTF-8 string. If the key is not present, the empty string must be returned.

#### Global Keys

Global Keys must be made up of lowercase letters, numbers and the hyphen (-).

* **avatar** - a URL to an image used as an avatar or logo
* **description** - A description of the name
* **display** - a canonical display name for the ENS name; this MUST match the ENS name when its case is folded, and clients should ignore this value if it does not (e.g. `"ricmoo.eth"` could set this to `"RicMoo.eth"`)
* **email** - an e-mail address
* **keywords** - A list of comma-separated keywords, ordered by most significant first; clients that interpresent this field may choose a threshold beyond which to ignore
* **mail** - A physical mailing address
* **notice** - A notice regarding this name
* **location** - A generic location (e.g. `"Toronto, Canada"`)
* **phone** - A phone number as an E.164 string
* **url** - a website URL

#### Service Keys

Service Keys must be made up of a _reverse dot notation_ for a namespace which the service owns, for example, DNS names (e.g. `.com`, `.io`, etc) or ENS name (i.e. `.eth`). Service Keys must contain at least one dot.

This allows new services to start using their own keys without worrying about colliding with existing services and also means new services do not need to update this document.

The following services are common, which is why recommendations are provided here, but ideally a service would declare its own key.

* **com.github** - a GitHub username
* **com.peepeth** - a Peepeth username
* **com.linkedin** - a LinkedIn username
* **com.twitter** - a Twitter username
* **io.keybase** - a Keybase username
* **org.telegram** - a Telegram username

This technique also allows for a service owner to specify a hierarchy for their keys, such as:

* **com.example.users**
* **com.example.groups**
* **com.example.groups.public**
* **com.example.groups.private**

#### Legacy Keys

The following keys were specified in earlier versions of this ENSIP.

Their use is not likely very wide, but applications attempting maximal compatibility may wish to query these keys as a fallback if the above replacement keys fail.

* **vnd.github** - a GitHub username (renamed to `com.github`)
* **vnd.peepeth** - a peepeth username (renamced to `com.peepeth`)
* **vnd.twitter** - a twitter username (renamed to `com.twitter`)

### Rationale

#### Application-specific vs general-purpose record types

Rather than define a large number of specific record types (each for generally human-readable data) such as `url` and `email`, we follow an adapted model of DNS's `TXT` records, which allow for a general keys and values, allowing future extension without adjusting the resolver, while allowing applications to use custom keys for their own purposes.

### Backwards Compatibility

Not applicable.

### Security Considerations

None.

### Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
