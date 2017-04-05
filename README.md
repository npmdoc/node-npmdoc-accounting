# api documentation for  [accounting (v0.4.1)](http://openexchangerates.github.io/accounting.js)  [![npm package](https://img.shields.io/npm/v/npmdoc-accounting.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-accounting) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-accounting.svg)](https://travis-ci.org/npmdoc/node-npmdoc-accounting)
#### number, money and currency formatting library

[![NPM](https://nodei.co/npm/accounting.png?downloads=true)](https://www.npmjs.com/package/accounting)

[![apidoc](https://npmdoc.github.io/node-npmdoc-accounting/build/screenCapture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-accounting_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-accounting/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-accounting/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-accounting/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "Open Exchange Rates",
        "email": "info@openexchangerates.org",
        "url": "https://www.openexchangerates.org"
    },
    "bugs": {
        "url": "https://github.com/openexchangerates/accounting.js/issues"
    },
    "contributors": [
        {
            "name": "Open Exchange Rates",
            "email": "info@openxchangerates.org",
            "url": "https://openexchangerates.org"
        },
        {
            "name": "Joss Crowcroft",
            "email": "josscrowcroft@gmail.com",
            "url": "http://www.josscrowcroft.com"
        }
    ],
    "dependencies": {},
    "description": "number, money and currency formatting library",
    "devDependencies": {},
    "directories": {},
    "dist": {
        "shasum": "87dd4103eff7f4460f1e186f5c677ed6cf566883",
        "tarball": "https://registry.npmjs.org/accounting/-/accounting-0.4.1.tgz"
    },
    "gitHead": "9b73d760f20fb5bd87a570f7b7ecf9af9cf5b76c",
    "homepage": "http://openexchangerates.github.io/accounting.js",
    "keywords": [
        "accounting",
        "number",
        "money",
        "currency",
        "format",
        "utilities",
        "finance",
        "exchange"
    ],
    "main": "accounting.js",
    "maintainers": [
        {
            "name": "joss",
            "email": "josscrowcroft@gmail.com"
        }
    ],
    "name": "accounting",
    "optionalDependencies": {},
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git://github.com/openexchangerates/accounting.js.git"
    },
    "scripts": {},
    "version": "0.4.1"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module accounting](#apidoc.module.accounting)
1.  [function <span class="apidocSignatureSpan">accounting.</span>format (number, precision, thousand, decimal)](#apidoc.element.accounting.format)
1.  [function <span class="apidocSignatureSpan">accounting.</span>formatColumn (list, symbol, precision, thousand, decimal, format)](#apidoc.element.accounting.formatColumn)
1.  [function <span class="apidocSignatureSpan">accounting.</span>formatMoney (number, symbol, precision, thousand, decimal, format)](#apidoc.element.accounting.formatMoney)
1.  [function <span class="apidocSignatureSpan">accounting.</span>formatNumber (number, precision, thousand, decimal)](#apidoc.element.accounting.formatNumber)
1.  [function <span class="apidocSignatureSpan">accounting.</span>parse (value, decimal)](#apidoc.element.accounting.parse)
1.  [function <span class="apidocSignatureSpan">accounting.</span>toFixed (value, precision)](#apidoc.element.accounting.toFixed)
1.  [function <span class="apidocSignatureSpan">accounting.</span>unformat (value, decimal)](#apidoc.element.accounting.unformat)
1.  object <span class="apidocSignatureSpan"></span>accounting
1.  object <span class="apidocSignatureSpan">accounting.</span>settings
1.  string <span class="apidocSignatureSpan">accounting.</span>version



# <a name="apidoc.module.accounting"></a>[module accounting](#apidoc.module.accounting)

#### <a name="apidoc.element.accounting.format"></a>[function <span class="apidocSignatureSpan">accounting.</span>format (number, precision, thousand, decimal)](#apidoc.element.accounting.format)
- description and source-code
```javascript
format = function (number, precision, thousand, decimal) {
		// Resursively format arrays:
		if (isArray(number)) {
			return map(number, function(val) {
				return formatNumber(val, precision, thousand, decimal);
			});
		}

		// Clean up number:
		number = unformat(number);

		// Build options object from second param (if object) or all params, extending defaults:
		var opts = defaults(
				(isObject(precision) ? precision : {
					precision : precision,
					thousand : thousand,
					decimal : decimal
				}),
				lib.settings.number
			),

			// Clean up precision
			usePrecision = checkPrecision(opts.precision),

			// Do some calc:
			negative = number < 0 ? "-" : "",
			base = parseInt(toFixed(Math.abs(number || 0), usePrecision), 10) + "",
			mod = base.length > 3 ? base.length % 3 : 0;

		// Format the number:
		return negative + (mod ? base.substr(0, mod) + opts.thousand : "") + base.substr(mod).replace(/(\d{3})(?=\d)/g, "$1" + opts.thousand
) + (usePrecision ? opts.decimal + toFixed(Math.abs(number), usePrecision).split('.')[1] : "");
	}
```
- example usage
```shell
...
		// Multiply up by precision, round accurately, then divide and use native toFixed():
		return (Math.round(lib.unformat(value) * power) / power).toFixed(precision);
	};


	/**
	 * Format a number, with comma-separated thousands and custom precision/decimal places
	 * Alias: 'accounting.format()'
	 *
	 * Localise by overriding the precision and thousand / decimal separators
	 * 2nd parameter 'precision' can be an object matching 'settings.number'
	 */
	var formatNumber = lib.formatNumber = lib.format = function(number, precision, thousand, decimal) {
		// Resursively format arrays:
		if (isArray(number)) {
...
```

#### <a name="apidoc.element.accounting.formatColumn"></a>[function <span class="apidocSignatureSpan">accounting.</span>formatColumn (list, symbol, precision, thousand, decimal, format)](#apidoc.element.accounting.formatColumn)
- description and source-code
```javascript
formatColumn = function (list, symbol, precision, thousand, decimal, format) {
		if (!list) return [];

		// Build options object from second param (if object) or all params, extending defaults:
		var opts = defaults(
				(isObject(symbol) ? symbol : {
					symbol : symbol,
					precision : precision,
					thousand : thousand,
					decimal : decimal,
					format : format
				}),
				lib.settings.currency
			),

			// Check format (returns object with pos, neg and zero), only need pos for now:
			formats = checkCurrencyFormat(opts.format),

			// Whether to pad at start of string or after currency symbol:
			padAfterSymbol = formats.pos.indexOf("%s") < formats.pos.indexOf("%v") ? true : false,

			// Store value for the length of the longest string in the column:
			maxLength = 0,

			// Format the list according to options, store the length of the longest string:
			formatted = map(list, function(val, i) {
				if (isArray(val)) {
					// Recursively format columns if list is a multi-dimensional array:
					return lib.formatColumn(val, opts);
				} else {
					// Clean up the value
					val = unformat(val);

					// Choose which format to use for this value (pos, neg or zero):
					var useFormat = val > 0 ? formats.pos : val < 0 ? formats.neg : formats.zero,

						// Format this value, push into formatted list and save the length:
						fVal = useFormat.replace('%s', opts.symbol).replace('%v', formatNumber(Math.abs(val), checkPrecision(opts.precision), opts
.thousand, opts.decimal));

					if (fVal.length > maxLength) maxLength = fVal.length;
					return fVal;
				}
			});

		// Pad each number in the list and send back the column of numbers:
		return map(formatted, function(val, i) {
			// Only if this is a string (not a nested array, which would have already been padded):
			if (isString(val) && val.length < maxLength) {
				// Depending on symbol position, pad after symbol or at index 0:
				return padAfterSymbol ? val.replace(opts.symbol, opts.symbol+(new Array(maxLength - val.length + 1).join(" "))) : (new Array
(maxLength - val.length + 1).join(" ")) + val;
			}
			return val;
		});
	}
```
- example usage
```shell
...
			// Store value for the length of the longest string in the column:
			maxLength = 0,

			// Format the list according to options, store the length of the longest string:
			formatted = map(list, function(val, i) {
				if (isArray(val)) {
					// Recursively format columns if list is a multi-dimensional array:
					return lib.formatColumn(val, opts);
				} else {
					// Clean up the value
					val = unformat(val);

					// Choose which format to use for this value (pos, neg or zero):
					var useFormat = val > 0 ? formats.pos : val < 0 ? formats.neg : formats.zero,
...
```

#### <a name="apidoc.element.accounting.formatMoney"></a>[function <span class="apidocSignatureSpan">accounting.</span>formatMoney (number, symbol, precision, thousand, decimal, format)](#apidoc.element.accounting.formatMoney)
- description and source-code
```javascript
formatMoney = function (number, symbol, precision, thousand, decimal, format) {
		// Resursively format arrays:
		if (isArray(number)) {
			return map(number, function(val){
				return formatMoney(val, symbol, precision, thousand, decimal, format);
			});
		}

		// Clean up number:
		number = unformat(number);

		// Build options object from second param (if object) or all params, extending defaults:
		var opts = defaults(
				(isObject(symbol) ? symbol : {
					symbol : symbol,
					precision : precision,
					thousand : thousand,
					decimal : decimal,
					format : format
				}),
				lib.settings.currency
			),

			// Check format (returns object with pos, neg and zero):
			formats = checkCurrencyFormat(opts.format),

			// Choose which format to use for this value:
			useFormat = number > 0 ? formats.pos : number < 0 ? formats.neg : formats.zero;

		// Return with currency symbol added:
		return useFormat.replace('%s', opts.symbol).replace('%v', formatNumber(Math.abs(number), checkPrecision(opts.precision), opts.
thousand, opts.decimal));
	}
```
- example usage
```shell
...
		return negative + (mod ? base.substr(0, mod) + opts.thousand : "") + base.substr(mod).replace(/(\d{3})(?=\d)/g, "$1" + opts.thousand
) + (usePrecision ? opts.decimal + toFixed(Math.abs(number), usePrecision).split('.')[1] : "");
	};


	/**
	 * Format a number into currency
	 *
	 * Usage: accounting.formatMoney(number, symbol, precision, thousandsSep, decimalSep, format)
	 * defaults: (0, "$", 2, ",", ".", "%s%v")
	 *
	 * Localise by overriding the symbol, precision, thousand / decimal separators and format
	 * Second param can be an object matching 'settings.currency' which is the easiest way.
	 *
	 * To do: tidy up the parameters
	 */
...
```

#### <a name="apidoc.element.accounting.formatNumber"></a>[function <span class="apidocSignatureSpan">accounting.</span>formatNumber (number, precision, thousand, decimal)](#apidoc.element.accounting.formatNumber)
- description and source-code
```javascript
formatNumber = function (number, precision, thousand, decimal) {
		// Resursively format arrays:
		if (isArray(number)) {
			return map(number, function(val) {
				return formatNumber(val, precision, thousand, decimal);
			});
		}

		// Clean up number:
		number = unformat(number);

		// Build options object from second param (if object) or all params, extending defaults:
		var opts = defaults(
				(isObject(precision) ? precision : {
					precision : precision,
					thousand : thousand,
					decimal : decimal
				}),
				lib.settings.number
			),

			// Clean up precision
			usePrecision = checkPrecision(opts.precision),

			// Do some calc:
			negative = number < 0 ? "-" : "",
			base = parseInt(toFixed(Math.abs(number || 0), usePrecision), 10) + "",
			mod = base.length > 3 ? base.length % 3 : 0;

		// Format the number:
		return negative + (mod ? base.substr(0, mod) + opts.thousand : "") + base.substr(mod).replace(/(\d{3})(?=\d)/g, "$1" + opts.thousand
) + (usePrecision ? opts.decimal + toFixed(Math.abs(number), usePrecision).split('.')[1] : "");
	}
```
- example usage
```shell
...
* **[money.js](http://openexchangerates.github.com/money.js "JavaScript and NodeJS Currency Conversion Library")** - a tiny (1kb
) standalone JavaScript currency conversion library, for web & nodeJS
* **[Open Exchange Rates](https://openexchangerates.org "realtime and historical exchange rates/currency conversion data API")** -
the free currency conversion data API

---

## Changelog

**v0.4.1** - Alias 'accounting.formatNumber()' as 'accounting.format()'

**v0.4** - Transferred repository to Open Exchange Rates for ongoing maintenance

**v0.3.2** - Fixed package.json dependencies (should be empty object)

**v0.3.0**
* Rewrote library structure similar to underscore.js for use as a nodeJS/npm and AMD module. Use 'npm install accounting' and then
 'var accounting = require("accounting");' in your nodeJS scripts.
...
```

#### <a name="apidoc.element.accounting.parse"></a>[function <span class="apidocSignatureSpan">accounting.</span>parse (value, decimal)](#apidoc.element.accounting.parse)
- description and source-code
```javascript
parse = function (value, decimal) {
		// Recursively unformat arrays:
		if (isArray(value)) {
			return map(value, function(val) {
				return unformat(val, decimal);
			});
		}

		// Fails silently (need decent errors):
		value = value || 0;

		// Return the value as-is if it's already a number:
		if (typeof value === "number") return value;

		// Default decimal point comes from settings, but could be set to eg. "," in opts:
		decimal = decimal || lib.settings.number.decimal;

		 // Build regex to strip out everything except digits, decimal point and minus sign:
		var regex = new RegExp("[^0-9-" + decimal + "]", ["g"]),
			unformatted = parseFloat(
				("" + value)
				.replace(/\((.*)\)/, "-$1") // replace bracketed values with negatives
				.replace(regex, '')         // strip out any cruft
				.replace(decimal, '.')      // make sure decimal point is standard
			);

		// This will fail silently which may cause trouble, let's wait and see:
		return !isNaN(unformatted) ? unformatted : 0;
	}
```
- example usage
```shell
...
	}


	/* --- API Methods --- */

	/**
	 * Takes a string/array of strings, removes all formatting/cruft and returns the raw float value
	 * Alias: 'accounting.parse(string)'
	 *
	 * Decimal must be included in the regular expression to match floats (defaults to
	 * accounting.settings.number.decimal), so if the number uses a non-standard decimal
	 * separator, provide it as the second argument.
	 *
	 * Also matches bracketed negatives (eg. "$ (1.99)" => -1.99)
	 *
...
```

#### <a name="apidoc.element.accounting.toFixed"></a>[function <span class="apidocSignatureSpan">accounting.</span>toFixed (value, precision)](#apidoc.element.accounting.toFixed)
- description and source-code
```javascript
toFixed = function (value, precision) {
		precision = checkPrecision(precision, lib.settings.number.precision);
		var power = Math.pow(10, precision);

		// Multiply up by precision, round accurately, then divide and use native toFixed():
		return (Math.round(lib.unformat(value) * power) / power).toFixed(precision);
	}
```
- example usage
```shell
...
		return !isNaN(unformatted) ? unformatted : 0;
	};


	/**
	 * Implementation of toFixed() that treats floats more like decimals
	 *
	 * Fixes binary rounding issues (eg. (0.615).toFixed(2) === "0.61") that present
	 * problems for accounting- and finance-related software.
	 */
	var toFixed = lib.toFixed = function(value, precision) {
		precision = checkPrecision(precision, lib.settings.number.precision);
		var power = Math.pow(10, precision);

		// Multiply up by precision, round accurately, then divide and use native toFixed():
...
```

#### <a name="apidoc.element.accounting.unformat"></a>[function <span class="apidocSignatureSpan">accounting.</span>unformat (value, decimal)](#apidoc.element.accounting.unformat)
- description and source-code
```javascript
unformat = function (value, decimal) {
		// Recursively unformat arrays:
		if (isArray(value)) {
			return map(value, function(val) {
				return unformat(val, decimal);
			});
		}

		// Fails silently (need decent errors):
		value = value || 0;

		// Return the value as-is if it's already a number:
		if (typeof value === "number") return value;

		// Default decimal point comes from settings, but could be set to eg. "," in opts:
		decimal = decimal || lib.settings.number.decimal;

		 // Build regex to strip out everything except digits, decimal point and minus sign:
		var regex = new RegExp("[^0-9-" + decimal + "]", ["g"]),
			unformatted = parseFloat(
				("" + value)
				.replace(/\((.*)\)/, "-$1") // replace bracketed values with negatives
				.replace(regex, '')         // strip out any cruft
				.replace(decimal, '.')      // make sure decimal point is standard
			);

		// This will fail silently which may cause trouble, let's wait and see:
		return !isNaN(unformatted) ? unformatted : 0;
	}
```
- example usage
```shell
...
	 * problems for accounting- and finance-related software.
	 */
	var toFixed = lib.toFixed = function(value, precision) {
		precision = checkPrecision(precision, lib.settings.number.precision);
		var power = Math.pow(10, precision);

		// Multiply up by precision, round accurately, then divide and use native toFixed():
		return (Math.round(lib.unformat(value) * power) / power).toFixed(precision);
	};


	/**
	 * Format a number, with comma-separated thousands and custom precision/decimal places
	 * Alias: 'accounting.format()'
	 *
...
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
