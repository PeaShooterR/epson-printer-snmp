# epson-printer-snmp

[![GitHub license](https://img.shields.io/github/license/Zedeldi/epson-printer-snmp?style=flat-square)](https://github.com/Zedeldi/epson-printer-snmp/blob/master/LICENSE) [![GitHub last commit](https://img.shields.io/github/last-commit/Zedeldi/epson-printer-snmp?style=flat-square)](https://github.com/Zedeldi/epson-printer-snmp/commits) [![Code style: black](https://img.shields.io/badge/code%20style-black-000000.svg?style=flat-square)](https://github.com/psf/black)

Read information and reset waste ink counters on Epson printers, using SNMP.

## Description

This project is a fork of [Zedeldi/epson-printer-snmp](https://github.com/Zedeldi/epson-printer-snmp) and modified for EPSON PX-047A Series printer.

If you would like to understand what and why I made modifications, it is advisable to read the original project's readme before reading this.

The format for reading values is:

`{eeprom_link}.124.124.7.0.{password}.65.190.160.{oid}.0`

The format for setting values is:

`{eeprom_link}.124.124.16.0.{password}.66.189.33.{oid}.0.{value}.78.118.116.100.98.115.106.47`

Where `eeprom_link` is consistently `1.3.6.1.4.1.1248.1.2.2.44.1.1.2.1` and `password` is two values, e.g. `101.0`, which seem to vary between different models of printer. This can be found by using a tool, such as `wicreset`, and checking the request it sends.
A method for brute forcing the password is provided in `Session.brute_force`, which tries to get a value from the EEPROM, for every permutation of `[0x00, 0x00]` to `[0xFF, 0xFF]`.

Setting values is done by *getting* an address, where the OID and value to set is specified in the query.
Certain values of these formats also vary between models of printer.

Various methods are defined to get specific information.
The `Printer.stats` method will return a dictionary of most useful information.

Check this for [how PX-047A store waste ink levels](https://github.com/Zedeldi/epson-printer-snmp/issues/1#issuecomment-1600061730).

## Usage
Edit `password: list[int] = field(default_factory=lambda: [85, 5])` to the password to your printer.

You can find it by using the `session.brute_force()`or checking the log of `wicreset` located at `%appdata%\wicreset\application.log`.

After that, run:

`python3 main.py <printer ip>`
## Libraries

- [easysnmp](https://pypi.org/project/easysnmp/) - SNMP

## Resources

If you want to modify this for your own printer, it is recommended to read [Zedeldi/epson-printer-snmp](https://github.com/Zedeldi/epson-printer-snmp).

## License

epson-printer-snmp is licensed under the GPL v3 for everyone to use, modify and share freely.

This program is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

[![GPL v3 Logo](https://www.gnu.org/graphics/gplv3-127x51.png)](https://www.gnu.org/licenses/gpl-3.0-standalone.html)

## Donate

If you found this project useful, please consider donating. Any amount is greatly appreciated! Thank you :smiley:

[![PayPal](https://www.paypalobjects.com/webstatic/mktg/Logo/pp-logo-150px.png)](https://paypal.me/ZackDidcott)

My bitcoin address is: [bc1q5aygkqypxuw7cjg062tnh56sd0mxt0zd5md536](bitcoin://bc1q5aygkqypxuw7cjg062tnh56sd0mxt0zd5md536)
