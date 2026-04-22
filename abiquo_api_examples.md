# Automating API Documentation: The Abiquo Wiki Examples Project

## Situation

Abiquo is an IaaS cloud management platform with a comprehensive REST API featuring over 100 resources and over 700 methods. The Abiquo wiki contains fully automated API reference documentation that replaced a previously manual version.

**Key Context:**

- Support staff work with multiple product versions simultaneously and need to switch between contexts quickly
- External developers require API examples to more easily understand the platform and design integrations  
- The Abiquo REST API uses HATEOAS (Hypermedia As The Engine Of Application State), which means navigation and object creation depend on links  
- API objects can have multiple dependencies—for example, launching a VM requires first creating a datacenter, then a virtual datacenter, with links to a cloud location  
- Links in Abiquo are complex, containing four components: URL, media type, relation, and title  
- Popular API documentation automation solutions did not provide adequate support for link documentation

## Objective

Create an automated system to generate and maintain over 700 API request examples that:

- Provide real-world examples extracted from actual integration tests  
- Support documentation of complex HATEOAS links and their relationships  
- Enable easy version-to-version comparison and updates  
- Reduce manual maintenance effort while maintaining quality control  
- Make the Abiquo API more visible to both support staff and external developers

## Action

The solution involved a three-stage automated workflow:

### Stage 1: Extract examples from untegration tests

- Developers generated request log files in JSON format as part of the build process  
- Created a Python script to process the log file and filter requests based on status code and other criteria  
- The script produced example files organized in the local file system  
- Files were named using abbreviations of request methods and media types. For example `GET_adm_ents_X_AC_ent_j` for GET requests retrieving an enterprise in JSON format)
- Multiple successful requests for the same query had sequence numbers

### Stage 2: Generate wiki pages

- Created a second Python script to take the list of example files and create or update individual wiki pages for each example  
- This automation allowed for easy updates across all examples with each release

### Stage 3: Manual review and integration

- Manually edited each API method section and inserted examples using an Include page macro  
- This review step ensured quality control—checking for issues like unusual test data or empty lists  
- Manually selected better example files to replace problematic ones
- After initial setup, automatic updates became straightforward for subsequent releases

**Implementation Details:**

- The Python scripts were version-controlled in the `abiquo-wiki-examples` GitHub repository (mjsmyth account)  
- The process enabled side-by-side comparison of examples between different API versions  
- The new API documentation extracts details of the most commonly used links from integration test results, providing better link coverage than previous solutions

## Result

The automation project successfully delivered:

- **700+ maintained examples** with minimal manual effort  
- **Improved documentation quality** that specifically addressed HATEOAS link documentation—a gap in existing automation solutions  
- **Better support for the Support team**, who could now more easily access examples across multiple product versions  
- **Increased API accessibility** for external developers learning to integrate with Abiquo  
- **Sustainable workflow** that made it easy to update examples with each new release

**Evidence:**

- Legacy example page (Abiquo 4.4): [abiquo_api_example_page_format.md](./abiquo_api_example_page_format.md)
- [Github repository of user mjsmyth](https://github.com/mjsmyth/abiquo-wiki-examples)

This project automated documentation maintenance to transform it from a labor-intensive manual process into a scalable system that improved both quality and accessibility.  
