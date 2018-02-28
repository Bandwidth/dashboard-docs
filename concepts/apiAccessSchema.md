# Formal API Schema Access

We make WADL schema descriptions available.  They are useful but they cannot be completely trusted for three reasons...

* We use URL redirects that confuse things.
* We use programmatic control over payload elements that are not visible in the WADL schema
* The annotation of the payloads is incomplete and not completely wired in.

Therefore, they are largely correct when available, but not perfect.  They can provide a shortcut to code generation, but you may see differences between the WADL schema and the formal spec.

Please feel free to use these to speed your development, but please also be aware that there are permissions-based programmatic alterations that are made to the XML payloads for certain API calls that will not show up in the WADL schema for those resources.   Use carefully.


Any primary resource will expose its WADL schema if you use the queryparameter `?_wadl` as in `https://test.dashboard.bandwidth.com/api/accounts?_wadl`

In cases where we have employed redirection you should see that in the `<resources base="https://test.dashboard.bandwidth.com/api/inventory_search"></resources>` tag, although you may need to follow that chain for a couple of steps in some cases.

## Example WADL

<code class="get">GET</code>`https://test.dashboard.bandwidth.com/api/accounts/9500087/availableNumbers?_wadl`

```xml
<application
    xmlns="http://wadl.dev.java.net/2009/02"
    xmlns:xs="http://www.w3.org/2001/XMLSchema">
    <grammars></grammars>
    <resources base="https://test.dashboard.bandwidth.com/v1.0/inventory_search"></resources>
</application>
```

Dereferencing the <resources base link: `https://test.dashboard.bandwidth.com/v1.0/inventory_search?_wadl` the expected WADL resource

```xml
<application
xmlns="http://wadl.dev.java.net/2009/02"
xmlns:xs="http://www.w3.org/2001/XMLSchema">
<grammars>
  <xs:schema
    xmlns:xs="http://www.w3.org/2001/XMLSchema" attributeFormDefault="unqualified" elementFormDefault="unqualified">
    <xs:element name="BaseSearchResult" type="baseSearchResult"/>
    <xs:element name="City" type="citySearchResult"/>
    <xs:element name="CityResponse" type="citySearchResults"/>
    <xs:element name="CoveredRateCenter" type="coveredRateCenter"/>
    <xs:element name="CoveredRateCenters" type="coveredRateCenterSearchResponse"/>
    <xs:element name="Links" type="paginationLinks"/>
    <xs:element name="RateCenter" type="rateCenterSearchResult"/>
    <xs:element name="RateCenterResponse" type="rateCenterSearchResults"/>
    <xs:element name="ResponseStatus" type="ResponseStatus"/>
    <xs:element name="ResultItemForAvailableNpaNxx" type="resultItemForAvailableNpaNxx"/>
    <xs:element name="SearchError" type="searchError"/>
    <xs:element name="SearchResult" type="searchResult"/>
    <xs:element name="SearchResultForAvailableNpaNxx" type="searchResultForAvailableNpaNxx"/>
    <xs:element name="TelephoneNumberDetail" type="telephoneNumberDetail"/>
    <xs:complexType name="coveredRateCenterSearchResponse">
      <xs:complexContent>
        <xs:extension base="searchResponse">
          <xs:sequence>
            <xs:element minOccurs="0" name="TotalCount" type="xs:int"/>
            <xs:element minOccurs="0" ref="Links"/>
            <xs:element maxOccurs="unbounded" minOccurs="0" ref="CoveredRateCenter"/>
          </xs:sequence>
        </xs:extension>
      </xs:complexContent>
    </xs:complexType>
    <xs:complexType abstract="true" name="searchResponse">
      <xs:complexContent>
        <xs:extension base="AbstractResponse">
          <xs:sequence/>
        </xs:extension>
      </xs:complexContent>
    </xs:complexType>
    <xs:complexType abstract="true" name="AbstractResponse">
      <xs:sequence>
        <xs:element minOccurs="0" ref="ResponseStatus"/>
      </xs:sequence>
    </xs:complexType>
    <xs:complexType name="paginationLinks">
      <xs:sequence>
        <xs:element minOccurs="0" name="first" type="xs:string"/>
        <xs:element minOccurs="0" name="next" type="xs:string"/>
        <xs:element minOccurs="0" name="previous" type="xs:string"/>
        <xs:element minOccurs="0" name="last" type="xs:string"/>
      </xs:sequence>
    </xs:complexType>
    <xs:complexType name="coveredRateCenter">
      <xs:sequence>
        <xs:element minOccurs="0" name="Id" type="xs:int"/>
        <xs:element minOccurs="0" name="Name" type="xs:string"/>
        <xs:element minOccurs="0" name="Abbreviation" type="xs:string"/>
        <xs:element minOccurs="0" name="State" type="xs:string"/>
        <xs:element minOccurs="0" name="Lata" type="xs:int"/>
        <xs:element minOccurs="0" name="AvailableNumberCount" type="xs:int"/>
        <xs:element minOccurs="0" name="ZipCodes">
          <xs:complexType>
            <xs:sequence>
              <xs:element maxOccurs="unbounded" minOccurs="0" name="ZipCode" type="xs:string"/>
            </xs:sequence>
          </xs:complexType>
        </xs:element>
        <xs:element minOccurs="0" name="Cities">
          <xs:complexType>
            <xs:sequence>
              <xs:element maxOccurs="unbounded" minOccurs="0" name="City" type="xs:string"/>
            </xs:sequence>
          </xs:complexType>
        </xs:element>
        <xs:element minOccurs="0" name="Vendors">
          <xs:complexType>
            <xs:sequence>
              <xs:element maxOccurs="unbounded" minOccurs="0" name="Vendor" type="xs:string"/>
            </xs:sequence>
          </xs:complexType>
        </xs:element>
        <xs:element minOccurs="0" name="Tiers">
          <xs:complexType>
            <xs:sequence>
              <xs:element maxOccurs="unbounded" minOccurs="0" name="Tier" type="xs:byte"/>
            </xs:sequence>
          </xs:complexType>
        </xs:element>
        <xs:element minOccurs="0" name="NpaNxxXs">
          <xs:complexType>
            <xs:sequence>
              <xs:element maxOccurs="unbounded" minOccurs="0" name="NpaNxxX" type="xs:string"/>
            </xs:sequence>
          </xs:complexType>
        </xs:element>
        <xs:element minOccurs="0" name="Npas">
          <xs:complexType>
            <xs:sequence>
              <xs:element maxOccurs="unbounded" minOccurs="0" name="Npa" type="xs:string"/>
            </xs:sequence>
          </xs:complexType>
        </xs:element>
        <xs:element minOccurs="0" name="LocalRateCenters">
          <xs:complexType>
            <xs:sequence>
              <xs:element maxOccurs="unbounded" minOccurs="0" name="RateCenterId" type="xs:int"/>
            </xs:sequence>
          </xs:complexType>
        </xs:element>
      </xs:sequence>
    </xs:complexType>
    <xs:complexType name="ResponseStatus">
      <xs:sequence>
        <xs:element minOccurs="0" name="ErrorCode" type="xs:int"/>
        <xs:element minOccurs="0" name="Description" type="xs:string"/>
      </xs:sequence>
    </xs:complexType>
    <xs:complexType name="citySearchResults">
      <xs:complexContent>
        <xs:extension base="searchResult">
          <xs:sequence>
            <xs:element minOccurs="0" name="Cities">
              <xs:complexType>
                <xs:sequence>
                  <xs:element maxOccurs="unbounded" minOccurs="0" ref="City"/>
                </xs:sequence>
              </xs:complexType>
            </xs:element>
          </xs:sequence>
        </xs:extension>
      </xs:complexContent>
    </xs:complexType>
    <xs:complexType name="searchResult">
      <xs:complexContent>
        <xs:extension base="baseSearchResult">
          <xs:sequence>
            <xs:element minOccurs="0" name="ResultCount" type="xs:int"/>
            <xs:element minOccurs="0" name="TelephoneNumberList">
              <xs:complexType>
                <xs:sequence>
                  <xs:element maxOccurs="unbounded" minOccurs="0" name="TelephoneNumber" type="xs:string"/>
                </xs:sequence>
              </xs:complexType>
            </xs:element>
            <xs:element minOccurs="0" name="TelephoneNumberDetailList">
              <xs:complexType>
                <xs:sequence>
                  <xs:element maxOccurs="unbounded" minOccurs="0" ref="TelephoneNumberDetail"/>
                </xs:sequence>
              </xs:complexType>
            </xs:element>
          </xs:sequence>
        </xs:extension>
      </xs:complexContent>
    </xs:complexType>
    <xs:complexType abstract="true" name="baseSearchResult">
      <xs:sequence>
        <xs:element minOccurs="0" name="Error" type="searchError"/>
      </xs:sequence>
    </xs:complexType>
    <xs:complexType name="citySearchResult">
      <xs:sequence>
        <xs:element minOccurs="0" name="RcAbbreviation" type="xs:string"/>
        <xs:element minOccurs="0" name="AvailableTelephoneNumberCount" type="xs:int"/>
        <xs:element minOccurs="0" name="Name" type="xs:string"/>
      </xs:sequence>
    </xs:complexType>
    <xs:complexType name="telephoneNumberDetail">
      <xs:sequence>
        <xs:element minOccurs="0" name="City" type="xs:string"/>
        <xs:element minOccurs="0" name="isTollfree" type="xs:boolean"/>
        <xs:element minOccurs="0" name="LATA" type="xs:string"/>
        <xs:element minOccurs="0" name="RateCenter" type="xs:string"/>
        <xs:element minOccurs="0" name="State" type="xs:string"/>
        <xs:element minOccurs="0" name="FullNumber" type="xs:string"/>
        <xs:element minOccurs="0" name="Tier" type="xs:int"/>
        <xs:element minOccurs="0" name="VendorId" type="xs:int"/>
        <xs:element minOccurs="0" name="VendorName" type="xs:string"/>
      </xs:sequence>
    </xs:complexType>
    <xs:complexType name="searchError">
      <xs:sequence>
        <xs:element name="Code" type="xs:int"/>
        <xs:element minOccurs="0" name="Description" type="xs:string"/>
      </xs:sequence>
    </xs:complexType>
    <xs:complexType name="searchResultForAvailableNpaNxx">
      <xs:complexContent>
        <xs:extension base="baseSearchResult">
          <xs:sequence>
            <xs:element minOccurs="0" name="AvailableNpaNxxList">
              <xs:complexType>
                <xs:sequence>
                  <xs:element maxOccurs="unbounded" minOccurs="0" name="AvailableNpaNxx" type="resultItemForAvailableNpaNxx"/>
                </xs:sequence>
              </xs:complexType>
            </xs:element>
          </xs:sequence>
        </xs:extension>
      </xs:complexContent>
    </xs:complexType>
    <xs:complexType name="resultItemForAvailableNpaNxx">
      <xs:sequence>
        <xs:element minOccurs="0" name="City" type="xs:string"/>
        <xs:element minOccurs="0" name="Npa" type="xs:string"/>
        <xs:element minOccurs="0" name="Nxx" type="xs:string"/>
        <xs:element name="Quantity" type="xs:int"/>
        <xs:element minOccurs="0" name="State" type="xs:string"/>
      </xs:sequence>
    </xs:complexType>
    <xs:complexType name="rateCenterSearchResults">
      <xs:complexContent>
        <xs:extension base="searchResult">
          <xs:sequence>
            <xs:element minOccurs="0" name="RateCenters">
              <xs:complexType>
                <xs:sequence>
                  <xs:element maxOccurs="unbounded" minOccurs="0" ref="RateCenter"/>
                </xs:sequence>
              </xs:complexType>
            </xs:element>
          </xs:sequence>
        </xs:extension>
      </xs:complexContent>
    </xs:complexType>
    <xs:complexType name="rateCenterSearchResult">
      <xs:sequence>
        <xs:element minOccurs="0" name="Abbreviation" type="xs:string"/>
        <xs:element minOccurs="0" name="AvailableTelephoneNumberCount" type="xs:int"/>
        <xs:element minOccurs="0" name="Name" type="xs:string"/>
      </xs:sequence>
    </xs:complexType>
  </xs:schema>
</grammars>
<resources base="https://test.dashboard.bandwidth.com/v1.0/inventory_search">
  <resource path="/">
    <resource path="accounts/{accountId}/availableNpaNxx">
      <param name="accountId" style="template" type="xs:int"/>
      <method name="GET">
        <request>
          <param name="areaCode" style="query" type="xs:string"/>
          <param name="state" style="query" type="xs:string"/>
          <param name="quantity" style="query" type="xs:int"/>
        </request>
        <response>
          <representation mediaType="application/xml"/>
        </response>
      </method>
    </resource>
    <resource path="accounts/{accountId}/availableNumbers">
      <param name="accountId" style="template" type="xs:int"/>
      <method name="GET">
        <request>
          <param name="orderBy" style="query" type="xs:string"/>
          <param name="tollFreeWildChar" style="query" type="xs:string"/>
          <param name="tollFreeVanity" style="query" type="xs:string"/>
          <param name="protectedSearchAttribute" style="query" type="xs:string"/>
          <param name="npaNxx" style="query" type="xs:string"/>
          <param name="npaNxxX" style="query" type="xs:string"/>
          <param name="zip" style="query" type="xs:string"/>
          <param name="city" style="query" type="xs:string"/>
          <param name="rateCenter" style="query" type="xs:string"/>
          <param name="lata" style="query" type="xs:string"/>
          <param name="quantity" style="query" type="xs:int"/>
          <param name="npa" style="query" type="xs:string"/>
          <param name="localVanity" style="query" type="xs:string"/>
          <param name="endsIn" style="query" type="xs:boolean"/>
          <param name="lca" style="query" type="xs:boolean"/>
          <param name="enableTnDetail" style="query" type="xs:boolean"/>
          <param name="state" style="query" type="xs:string"/>
        </request>
        <response>
          <representation mediaType="application/xml"/>
          <representation mediaType="application/json"/>
        </response>
      </method>
    </resource>
    <resource path="cities">
      <method name="GET">
        <request>
          <param name="state" style="query" type="xs:string"/>
          <param name="supported" style="query" type="xs:boolean"/>
          <param name="available" style="query" type="xs:boolean"/>
        </request>
        <response>
          <representation mediaType="application/xml"/>
        </response>
      </method>
    </resource>
    <resource path="coveredRateCenters">
      <method name="GET">
        <request>
          <param name="npaNxx" style="query" type="xs:string"/>
          <param name="npaNxxX" style="query" type="xs:string"/>
          <param name="zip" style="query" type="xs:string"/>
          <param name="city" style="query" type="xs:string"/>
          <param name="tier" style="query" type="xs:string"/>
          <param name="abbreviation" style="query" type="xs:string"/>
          <param name="lata" style="query" type="xs:string"/>
          <param name="npa" style="query" type="xs:string"/>
          <param name="embedFields" style="query" repeating="true" type="xs:string"/>
          <param name="state" style="query" type="xs:string"/>
          <param name="name" style="query" type="xs:string"/>
          <param name="page" style="query" type="xs:string"/>
          <param name="size" style="query" type="xs:int"/>
        </request>
        <response>
          <representation mediaType="application/xml"/>
          <representation mediaType="application/json"/>
        </response>
      </method>
    </resource>
    <resource path="coveredRateCenters/{rateCenterId}">
      <param name="rateCenterId" style="template" type="xs:int"/>
      <method name="GET">
        <request/>
        <response>
          <representation mediaType="application/xml"/>
          <representation mediaType="application/json"/>
        </response>
      </method>
    </resource>
    <resource path="rateCenters">
      <method name="GET">
        <request>
          <param name="state" style="query" type="xs:string"/>
          <param name="supported" style="query" type="xs:boolean"/>
          <param name="available" style="query" type="xs:boolean"/>
        </request>
        <response>
          <representation mediaType="application/xml"/>
        </response>
      </method>
    </resource>
    <resource path="typeahead">
      <method name="GET">
        <request>
          <param name="q" style="query" type="xs:string"/>
          <param name="type" style="query" type="xs:string"/>
        </request>
        <response>
          <representation mediaType="application/json"/>
          <representation mediaType="application/xml"/>
        </response>
      </method>
    </resource>
  </resource>
</resources>undefined</application>
```