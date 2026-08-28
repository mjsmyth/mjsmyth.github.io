# Automating API documentation: the Abiquo API examples project

## 700 API examples were non-existent or out of date

Abiquo was an IaaS cloud management platform with a comprehensive RESTful API featuring over 100 resources and over 700 methods. The Abiquo wiki contained manually updated documentation.

- Support staff worked with multiple product versions simultaneously and need to switch between contexts
- External developers required API examples to understand the platform and design integrations  
- The Abiquo RESTful API used HATEOAS (Hypermedia As The Engine Of Application State), which means navigation and object creation depend on links  
- API objects could have multiple dependencies, for example, launching a VM requires first creating a data center, then a virtual data center, with links to a cloud location
- Links in Abiquo are complex, containing four components: URL, media type, relation, and title  
- Popular API documentation automation solutions didn't provide adequate support for link documentation

## No one wanted to update 700 examples by hand

Create an automated system to generate and maintain over 700 API request examples that:

- Provide real-world examples extracted from actual integration tests  
- Support documentation of complex HATEOAS links and their relationships  
- Enable easy version-to-version comparison and updates  
- Reduce manual maintenance effort while maintaining quality control  
- Make the Abiquo API more visible to both support staff and external developers

## I convinced developers to give me the unit test log

The solution involved a three-stage automated work flow.

### Stage 1: Get examples from integration tests

- Developers generated request log files in JSON format as part of the build process  
- I wrote a Python script to process the log file and filter requests based on status code and other criteria  
- The script produced example files organized in the local file system  
- Files names used abbreviations of request methods and media types. For example `GET_adm_ents_X_AC_ent_j` for GET requests retrieving an enterprise in JSON format)
- Multiple successful requests for the same query had sequence numbers in the filename

### Stage 2: Generate Confluence pages

- I wrote a second Python script to take the list of example files and create or update individual Confluence wiki pages for each example  
- This automation allowed for easy updates across all examples with each release

### Stage 3: Manual review and integration

- I manually edited each API method section in Confluence and inserted examples using an Include page macro 
- This review step ensured quality control checking for issues like unusual test data or empty lists  
- Manually selected better example files to replace problematic ones
- After initial setup, automatic updates became straightforward for subsequent releases

Implementation details:

- The Python scripts were version-controlled in the `abiquo-wiki-examples` GitHub repository in mjsmyth account  
- The process enabled side-by-side comparison of examples between different API versions 
- The new automated API documentation extracted details of the most commonly used links from integration test results, providing better link coverage than previous solutions

## I had automated the API examples

The automation project successfully delivered:

- **700+ maintained examples** with minimal manual effort
- **Improved documentation quality** that specifically addressed HATEOAS link documentation not available in existing documentation solutions
- **Better living for the Support team**, who could access examples across multiple product versions
- **Increased API accessibility** for external developers learning to integrate with Abiquo
- **Sustainable work flow** that made it easy to update examples with each new release
- **Incorporated into automated API docs** when the dev team (finally) implemented a custom API docs solution

**Example page and python script:**

- Legacy example page from Abiquo 4.4: [abiquo_api_example_page_format.md](./abiquo_api_example_page_format.md)
- [Github repository of user mjsmyth](https://github.com/mjsmyth/abiquo-wiki-examples)

This project automated documentation maintenance to transform it from an annoying manual process into a scalable system that improved both quality and accessibility of the documentation as a whole.

