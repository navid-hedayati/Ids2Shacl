# Ids2Shacl

Ids2Shacl is a library that provides tools to automatically IDS files which are expressed in xml format into SHACL shapes for valiating building information requirements.

## What is IDS (Information Delivery Specification)?

[The Information Delivery Specification (IDS)](https://www.buildingsmart.org/what-is-information-delivery-specification-ids/) is a standard in development from [buildingSMART](https://www.buildingsmart.org/) for defining information requirements in a way that is easily read by humans and interpreted by computers.

This standard in development helps people in the built asset industry to better define their exchange requirements and adds clarity amongst various stakeholders. It ensures asset owners can specify accurately what they want, and allows project participants with a better insight into what they need to deliver. It adds certainty and clarity when used in combination with other standards and services.

Traditionally the information requirements are shared in ways that are not computer interpretable, such as excel sheets or PDFs and it is very difficult for the right people to access the data that is pertinent to them for a particular situation.

Users that want to use data in the built asset industry usually know what information they need for a task but don’t always know how to specify it. This can have a big impact on things like automated code compliance checking, which requires different information than automated cost estimation, for example.

With IDS a user can require the use of properties (including quantities and attributes), materials, classifications, entity types and object dependency (partOf). At the time of writing this, IDS does not have the capability to define the details of geometry.

## An IDS example

Let’s say that a user wants all spaces in a model to be classified with a certain code and have a couple of properties.

The could be described as “All Space data in a model shall be classified as [AT]Zimmer and have NetFloorArea and GrossFloorArea (both in set called ‘BaseQuanitites’) and a property called AT_Zimmernummer in the property set Austria_example.

This is only an example. It could be any requirement. Users can also further refine requirements to not apply to all spaces but only to spaces with certain characteristics. For example, spaces with a certain property and/or property value, or spaces that are part of a certain hierarchy, or spaces that are classified in a certain way.

This goes for all objects, not only spaces. The requirements of classification codes, materials, quantities, attributes, properties, materials and some relations can also be set to the selection (sometimes also called filtering; formally called applicability in IDS) of objects.

Back to our example. Formatting this human-readable requirement in an IDS looks like this:

```xml
 <?xml version='1.0' encoding='utf-8'?>
<ids xmlns="http://standards.buildingsmart.org/IDS" xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://standards.buildingsmart.org/IDS ids.xsd">   
    <info>
        <title>buildingSMART Sample IDS</title>
        <copyright>buildingSMART</copyright>
        <version>1.0.0</version>
        <description>These are example specifications for those learning how to use IDS</description>
        <author>foo@bar.com</author>
        <date>2022-01-01</date>
        <purpose>Contractual requirements</purpose>
    </info>
    <specifications>
        <specification name="Project naming" ifcVersion="IFC4" description="Projects shall be named correctly for the purposes of identification, project archival, and model federation." instructions="Each discipline is responsible for naming their own project." minOccurs="0" maxOccurs="unbounded">
            <applicability>
                <entity>
                    <name>
                        <simpleValue>IFCPROJECT</simpleValue>
                    </name>
                </entity>
            </applicability>
            <requirements>
                <attribute instructions="The project manager shall confirm the short project code with the client based on their real estate portfolio naming scheme.">
                    <name>
                        <simpleValue>Name</simpleValue>
                    </name>
                    <value>
                        <simpleValue>TEST</simpleValue>
                    </value>
                </attribute>
            </requirements>
        </specification>
        <specification name="Fire rating" ifcVersion="IFC4" description="All objects must have a fire rating for building compliance checks and to know the protection strategies needed for any penetrations." instructions="The architect is responsible for including this data." minOccurs="0" maxOccurs="unbounded">
            <applicability>
                <entity>
                    <name>
                        <simpleValue>IFCWALLTYPE</simpleValue>
                    </name>
                </entity>
            </applicability>
            <requirements>
                <property datatype="IfcLabel" instructions="Fire rating is specified using the Fire Resistance Level as defined in the Australian National Construction Code (NCC) 2019. Valid examples include -/-/-, -/120/120, and 60/60/60" minOccurs="1" maxOccurs="1">
                    <propertySet>
                        <simpleValue>Pset_WallCommon</simpleValue>
                    </propertySet>
                    <name>
                        <simpleValue>FireRating</simpleValue>
                    </name>
                    <value>
                        <xs:restriction base="xs:string">
                            <xs:pattern value="(-|[0-9]{2,3})\/(-|[0-9]{2,3})\/(-|[0-9]{2,3})" />
                        </xs:restriction>
                    </value>
                </property>
            </requirements>
        </specification>
    </specifications>
</ids>
 ```
