# Content packages
### Specification

Gaming content must be packed into a package. A content package contains:
- characters, weapons and other objects
- backgrounds
- sounds
- stages
- custom interface

## distribution
#### on the web
A package can be deployed on a different domain than the main application is deployed. This allows third parties to distribute game content without hassles on fixes and updates of the game program.
#### offline use
A package can also be zipped and be distributed as a single file, and unzipped when use.

## package format
A package is a directory on the web or a folder on disk. Content files must be placed under the package directory, with no more than 4 levels of subdirectories (currently, these restrictions are not enforced programmatically though).

At the root of the package, there must be a file named `manifest.js`, containing a strict JSON structure surrounded by a `define()`, like:
```
define({
	...JSON data...
});
```
note that the content between the first open braces `{` and the last closing braces `}` must be strict JSON, i.e. can be parsed with `JSON.parse()` without errors. this is the only JSON structure with strict requirement, other `.js` files can define _JSON like_ structures that are actually javascript object literals.

the JSON data in `manifest.js` is in the following schema:
```
{
	"data":"string/url-js",
	"properties":"string/url-js",
	"resourcemap/optional":"string/url-js"
}
```
> note that this JSON schema is made up by me for the sake of understandability. there is however plan to develop a validator of this format.

`url-js` is the url to a file with `.js` extension with the file name without the extension. sounds quirky, but this is inherited from `requirejs`. basically `path/to/x.js` should be written as `path/to/x`.

the entries are:
- `data` is the url to the file `data.js`, corresponds to `data.txt` as found in LF2
- `properties` is the url to the file `properties.js`, its format and usage is defined by __F.LF (extended standard)__
- `resourcemap` is the url to the file `resourcemap.js`, its format and usage is defined by [__F.core__](http://tyt2y3.github.io/F.core/docs/docs.html#resourcemap). a resourcemap allows mapping from a canonical resource name (shorter and understandable) to the actual url (long and ugly), for example, consider an entry:
`'squirrel.png':'http://imagehost.com/FtvJG6rAG2mdB8aHrEa8qXj8GtbYRpqrQs9F8X8.png'`

> these files can be arbitrarily named, but must have `.js` as extension.

all file paths defined in the package, including those in `data.js` or `bandit.js` is defined relative to the root of package where `manifest.js` is located. __do not__ define a web resource like `http://some-imagehost.com/bandit.png`. if you want to host files on CDN to accelerate loading time _on the web_, define a resource map.

there is however no restriction on how the folders must be structured, the recommendation is:
```
bg/
data/
sound/
sprite/
manifest.js
resourcemap.js
```

### example
it is always easier to learn from examples. check out the package `LF2_19` in the [release channel](https://github.com/tyt2y3/LFrelease/tree/master/LF2_19).
