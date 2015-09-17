﻿
# The GeoDCAT-AP API (GeoDCAT-API)

This API is a proof-of-concept of the implementation of GeoDCAT-AP in an [OGC CSW (Catalog Service for the Web)](http://www.opengeospatial.org/standards/cat), re-using the standard CSW interface, and supporting in addition [HTTP content negotiation](https://tools.ietf.org/html/rfc7231#section-3.4).

More precisely, GeoDCAT-API uses the standard CSW parameters `outputSchema` and `outputFormat` to determine, respectively, (a) the GeoDCAT-AP profile to be used (core or extended), and (b) the RDF serialisation to be returned.

The ISO 19139 records to be transformed is specified by a GeoDCAT-API-specific parameter `src`, which is not part of the CSW interface. 

The API uses the GeoDCAT-AP XSLT to transform ISO 19139 records into GeoDCAT-AP. As such, the API works both on static files including the records, and on the CSW output of a `GetRecords` or `GetRecordById` request.

A working demo of GeoDCAT-API is available at: 

http://geodcat-ap.semic.eu:8890/api/

# API specification

## Supported HTTP methods

The current version of GeoDCAT-API supports only the HTTP `GET` method. As a consequence, it can be used only on CSWs supporting `GET` requests.

## API parameters

<table>
  <thead>
    <tr>
      <th>Parameter</th>
      <th>Description</th>
      <th>Possible values</th>
      <th>Default value</th>
      <th>Notes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>ouputSchema</code></td>
      <td>The GeoDCAT-AP profile to be used for the transformation</td>
      <td>
        <ul>
          <li><code>core</code> (DCAT-AP)</li>
          <li><code>extended</code> (GeoDCAT-AP)</li>
       </ul>
      </td>
      <td><code>core</code></td>
      <td>
        <p>If this parameter is omitted, the API uses the "core" profile as default.</p>
        <p>The "core" profile is labelled "DCAT-AP", since it returns just the metadata elements supported in DCAT-AP.</p>
        <p><strong>NB</strong>: The current values of this parameter are provisional, and they are meant to be replaced by the official namespace URIs of DCAT-AP and GeoDCAT-AP, when available.</p>
      </td>
    </tr>
    <tr>
      <td><code>outputFormat</code></td>
      <td>The RDF serialisation to be returned</td>
      <td>
        <ul>
          <li><code>application/rdf+xml</code> (<a href="http://www.w3.org/TR/rdf-syntax-grammar/">RDF/XML</a>)</li>
          <li><code>text/turtle</code> (<a href="http://www.w3.org/TR/turtle/">Turtle</a>)</li>
          <li><code>text/n3</code> (<a href="http://www.w3.org/TeamSubmission/n3/">Notation 3</a>)</li>
          <li><code>application/n-triples</code> (<a href="http://www.w3.org/TR/n-triples/">N-Triples</a>)</li>
          <li><code>application/ld+json</code> (<a href="http://www.w3.org/TR/json-ld/">JSON-LD</a>)</li>
          <li><code>text/html</code> (<a href="http://www.w3.org/TR/html-rdfa/">HTML+RDFa</a>)</li>
       </ul>
      </td>
      <td>N/A</td>
      <td>If this parameter is omitted, the returned RDF serialisation is determined via HTTP content negotiation</td>
    </tr>
    <tr>
      <td><code>src</code></td>
      <td>The URL of the resource containing the ISO 19139 records to be tranformed</td>
      <td>A URL</td>
      <td>N/A</td>
      <td></td>
    </tr>
  </tbody>
</table>

# Implementation details

GeoDCAT-API is implemented in [PHP5](http://php.net/), and run on top of an [Apache 2 HTTP server](http://httpd.apache.org/).

The [EasyRDF](http://www.easyrdf.org/) and the [ML/JSON-LD](https://github.com/lanthaler/JsonLD) PHP libraries are used to generate the supported RDF serialisations. The HTML+RDFa serialisation is generated by using the [DCAT-AP in HTML+RDFa](../../../dcat-ap-rdf2html/) XSLT.

# Installation instructions

GeoDCAT-API has been tested on both Linux and Windows, with Apache 2 and PHP 5.3.2 (or later) installed and running.

**NB**: GeoDCAT-API makes use of the [PHP XSL extension](http://php.net/manual/en/xsl.installation.php).

The repository includes all what is necessary, with the exception of EasyRDF and ML/JSON-LD, that must be installed separately by using [Composer](https://getcomposer.org/).

More precisely:

* Go to folder [`./lib/composer/`](./lib/composer/).
* [Download Composer](https://getcomposer.org/download/). E.g.: `curl -s https://getcomposer.org/installer | php`
* Run `php composer.phar install`

You will now be able to run the API from a Web folder.