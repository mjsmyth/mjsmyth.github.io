# How to create a tenant via API

## Introduction

This page describes how to create a basic tenant (enterprise) and some users.  
You will need a datacenter or public cloud region.

Your user role must include the following privileges: 

* ENTERPRISE_ADMINISTER_ALL  
* MANAGE_ENTERPRISES  
* MANAGE_USERS  
* APPLIB_ALLOW_MODIFY

After you create a tenant, you can continue and add templates. See [How to add a VM template in a datacenter via API](#bookmark=id.2aqo3qhrhj2k)  
The following pages describe the enterprise UI and Apps library functionality:

* [Manage enterprises](#bookmark=id.2wbxvt8qbka)  
* [Configure an enterprise in a cloud location](#bookmark=id.98qrzpfq1kec)  
* [Manage users](#bookmark=id.g58ewup573k5)

And to add templates:

* [Add VM templates to the Apps library](#bookmark=id.cp31xqseqe7)  
* [Manage VM templates](#bookmark=id.u2ytbjw7fgzt)

## Summary diagram

The steps in this diagram link to the API reference guide resources and data entities sections.

```

                                          ( )
                                           |
                                           v
                        +----------------------------------+
                        | Requires: datacenter or          |
                        | public cloud region              |
                        +----------------------------------+
                                           |
                                           v
                        +----------------------------------+
                        | Create an enterprise object      |
                        +----------------------------------+
                                           |
                                           v
                        +----------------------------------+
                        | Create the enterprise            |
                        +----------------------------------+
                                           |
                                           v
                        +----------------------------------+
                        | Get a link to a datacenter or    |
                        | public cloud region for the      |
                        | enterprise to use                |
                        +----------------------------------+
                                   /              \
                                  v                v
                    +------------+          +----------------------+
                    | Datacenter |          | Public cloud region  |
                    +------------+          +----------------------+
                                   \              /
                                    v            v
                        +----------------------------------+
                        | Create a limit object            |
                        +----------------------------------+
                                           |
                                           v
                        +----------------------------------+
                        | Create a limit to allow access   |
                        | to the Datacenter or Public      |
                        | Cloud Region                     |
                        +----------------------------------+
                                           |
                                           v
                        +----------------------------------+
                        | Switch to the new enterprise     |
                        +----------------------------------+
                                           |
                                           v
                        +----------------------------------+
                        | Create user objects              |
                        +----------------------------------+
                                           |
                                           v
                        +----------------------------------+
                        | Create users                     |
                        +----------------------------------+
                                           |
                                           v
                        +----------------------------------+
                        | Add a VM template via API in DC  |
                        +----------------------------------+
                                           |
                                           v
                                         ( * )

```

## Detailed steps

1. **Create an enterprise object**  
   1. Reference: [Enterprise resource](https://wiki.abiquo.com/api/latest/enterprise.html)  
   2. If you use the following example, change the name and enterprise allocation limits as required  
      Sample enterpriseentity.json file

      ```json
      {
        "name" : "enterprise_444",
        "isReservationRestricted" : false,
        "workflow" : false,
        "twoFactorAuthenticationMandatory" : false,
        "reseller" : false,
        "keyNode" : false,
        "diskSoftLimitInMb" : 1024,
        "diskHardLimitInMb" : 4096,
        "vmsSoft" : 0,
        "vmsHard" : 0,
        "vlansSoft" : 0,
        "vlansHard" : 0,
        "publicIpsSoft" : 0,
        "publicIpsHard" : 0,
        "ramSoft" : 1,
        "ramHard" : 2,
        "cpuSoft" : 1,
        "cpuHard" : 2
      }
      ```

   3. Optionally set the reseller or keyNode attributes for an enterprise hierarchy  
2. **Create the enterprise**  
   * Reference: [Create an enterprise reference](https://wiki.abiquo.com/api/latest/EnterprisesResource.html#create-an-enterprise)  
   * cURL

     ```bash
     curl -X POST https://abiquoapi.com/api/admin/enterprises \
          -H 'Accept:application/vnd.abiquo.enterprise+json; version=4.7' \
          -H 'Content-Type:application/vnd.abiquo.enterprise+json; version=4.7' \
          -d @enterpriseentity.json \
          -u <USERNAME>:<PASSWORD> --verbose
     ```

   * Sample response. Success status code: 200  
     This request returns the enterprise

     ```json
     {  
         "id": 292,  
         "name": "enterprise_444",  
         "isReservationRestricted": false,  
         "workflow": false,  
         "twoFactorAuthenticationMandatory": false,  
         "reseller": false,  
         "keyNode": false,  
         "diskSoftLimitInMb": 1024,  
         "diskHardLimitInMb": 4096,  
         "storageSoftInMb": 0,  
         "storageHardInMb": 0,  
         "vmsSoft": 0,  
         "vmsHard": 0,  
         "vlansSoft": 0,  
         "vlansHard": 0,  
         "publicIpsSoft": 0,  
         "publicIpsHard": 0,  
         "repositorySoftInMb": 0,  
         "repositoryHardInMb": 0,  
         "links": [  
             {  
                 "title": "enterprise_444",  
                 "rel": "edit",  
                 "type": "application/vnd.abiquo.enterprise+json",  
                 "href": "https://example.com:443/api/admin/enterprises/292"  
             },  
             {  
                 "title": "users",  
                 "rel": "users",  
                 "type": "application/vnd.abiquo.users+json",  
                 "href": "https://example.com:443/api/admin/enterprises/292/users"  
             },  
             {  
                 "title": "properties",  
                 "rel": "properties",  
                 "type": "application/vnd.abiquo.enterpriseproperties+json",  
                 "href": "https://example.com:443/api/admin/enterprises/292/properties"  
             },  
             {  
                 "title": "Global scope",  
                 "rel": "scope",  
                 "type": "application/vnd.abiquo.scope+json",  
                 "href": "https://example.com:443/api/admin/scopes/1"  
             },  
             {  
                 "title": "template definitions lists",  
                 "rel": "appslib/templateDefinitionLists",  
                 "type": "application/vnd.abiquo.templatedefinitionlists+json",  
                 "href": "https://example.com:443/api/admin/enterprises/292/appslib/templateDefinitionLists"  
             },  
             {  
                 "title": "template defintion paths",  
                 "rel": "appslib/templateDefinitions",  
                 "type": "application/vnd.abiquo.templatedefinitions+json",  
                 "href": "https://example.com:443/api/admin/enterprises/292/appslib/templateDefinitions"  
             },  
             {  
                 "title": "repositories",  
                 "rel": "datacenterrepositories",  
                 "type": "application/vnd.abiquo.datacenterrepositories+json",  
                 "href": "https://example.com:443/api/admin/enterprises/292/datacenterrepositories"  
             },  
             {  
                 "title": "virtual machines",  
                 "rel": "virtualmachines",  
                 "type": "application/vnd.abiquo.virtualmachines+json",  
                 "href": "https://example.com:443/api/admin/enterprises/292/action/virtualmachines"  
             },  
             {  
                 "title": "virtual appliances",  
                 "rel": "virtualappliances",  
                 "type": "application/vnd.abiquo.virtualappliances+json",  
                 "href": "https://example.com:443/api/admin/enterprises/292/action/virtualappliances"  
             },  
             {  
                 "title": "ips",  
                 "rel": "ips",  
                 "type": "application/vnd.abiquo.privateips+json",  
                 "href": "https://example.com:443/api/admin/enterprises/292/action/ips"  
             },  
             {  
                 "title": "virtual datacenters",  
                 "rel": "cloud/virtualdatacenters",  
                 "type": "application/vnd.abiquo.virtualdatacenters+json",  
                 "href": "https://example.com:443/api/admin/enterprises/292/action/virtualdatacenters"  
             },  
             {  
                 "title": "reserved machines",  
                 "rel": "reservedmachines",  
                 "type": "application/vnd.abiquo.machines+json",  
                 "href": "https://example.com:443/api/admin/enterprises/292/reservedmachines"  
             },  
             {  
                 "title": "limits",  
                 "rel": "limits",  
                 "type": "application/vnd.abiquo.limits+json",  
                 "href": "https://example.com:443/api/admin/enterprises/292/limits"  
             },  
             {  
                 "title": "providerlimits",  
                 "rel": "providerlimits",  
                 "type": "application/vnd.abiquo.providerlimits+json",  
                 "href": "https://example.com:443/api/admin/enterprises/292/providerlimits"  
             },  
             {  
                 "title": "volumes",  
                 "rel": "volumes",  
                 "type": "application/vnd.abiquo.volumes+json",  
                 "href": "https://example.com:443/api/admin/enterprises/292/action/volumes"  
             },  
             {  
                 "title": "external networks",  
                 "rel": "externalnetworks",  
                 "type": "application/vnd.abiquo.vlans+json",  
                 "href": "https://example.com:443/api/admin/enterprises/292/action/externalnetworks"  
             },  
             {  
                 "title": "pending tasks",  
                 "rel": "pendingtasks",  
                 "type": "application/vnd.abiquo.tasks+json",  
                 "href": "https://example.com:443/api/admin/enterprises/292/action/pendingtasks"  
             },  
             {  
                 "title": "credentials",  
                 "rel": "credentials",  
                 "type": "application/vnd.abiquo.publiccloudcredentialslist+json",  
                 "href": "https://example.com:443/api/admin/enterprises/292/credentials"  
             },  
             {  
                 "title": "pricingcredentials",  
                 "rel": "pricingcredentials",  
                 "type": "application/vnd.abiquo.pricingcredentialslist+json",  
                 "href": "https://example.com:443/api/admin/enterprises/292/pricingcredentials"  
             },  
             {  
                 "title": "virtual appliances specs",  
                 "rel": "vappspecs",  
                 "type": "application/vnd.abiquo.virtualappliancespecs+json",  
                 "href": "https://example.com:443/api/admin/enterprises/292/vappspecs"  
             },  
             {  
                 "title": "bills",  
                 "rel": "bills",  
                 "type": "application/vnd.abiquo.bills+json",  
                 "href": "https://example.com:443/api/statistics/enterpriseresources/292/bills"  
             },  
             {  
                 "title": "billregisters",  
                 "rel": "billregisters",  
                 "type": "application/vnd.abiquo.bills+json",  
                 "href": "https://example.com:443/api/statistics/enterpriseresources/292/billregisters"  
             },  
             {  
                 "title": "estimation",  
                 "rel": "estimation",  
                 "type": "text/plain",  
                 "href": "https://example.com:443/api/statistics/enterpriseresources/292/billregisters/action/estimate"  
             },  
             {  
                 "title": "Bill providers",  
                 "rel": "billproviders",  
                 "type": "application/vnd.abiquo.billproviders+json",  
                 "href": "https://example.com:443/api/statistics/enterpriseresources/292/bills/action/providers"  
             },  
             {  
                 "title": "Budgets",  
                 "rel": "budgets",  
                 "type": "application/vnd.abiquo.budgets+json",  
                 "href": "https://example.com:443/api/admin/enterprises/292/budgets"  
             },  
             {  
                 "title": "Reseller enterprise",  
                 "rel": "reseller",  
                 "type": "application/vnd.abiquo.enterprise+json",  
                 "href": "https://example.com:443/api/admin/enterprises/267"  
             }  
         ],  
         "ramSoft": 1,  
         "ramHard": 2,  
         "cpuSoft": 1,  
         "cpuHard": 2  
     }
     ```

3. Keep the enterprise's edit link, to use when you switch to the enterprise

   ```json
   {
       "title": "enterprise_444",
       "rel": "edit",
       "type": "application/vnd.abiquo.enterprise+json",
       "href": "https://example.com:443/api/admin/enterprises/292"
   },
   ```

4. **Get a link to a datacenter or public cloud region for the enterprise to use**  
   1. Get the datacenter or public cloud region. References:  
      1. [List datacenter names and ids](https://wiki.abiquo.com/api/latest/DatacentersResource.html#list-datacenter-names-and-ids)  
      2. [List public cloud regions](https://wiki.abiquo.com/api/latest/PublicCloudRegionsResource.html#list-public-cloud-regions)  
   2. Obtain the href version of the datacenter or publiccloudregion link from the datacenters-light or publiccloudregion object. Use a `rel` value of `location`  
      1. datacenter link

         ```json
         {
             "title": "Abiquo-DC",
             "rel": "datacenter",
             "type": "application/vnd.abiquo.datacenter+json",
             "href": "https://example.com:443/api/admin/datacenters/1"
         }
         ```

      2. publiccloudregion link.

         ```json
         {
             "title": "000AmazonDC-IReland",
             "rel": "edit",
             "type": "application/vnd.abiquo.publiccloudregion+json",
             "href": "https://example.com:443/api/admin/publiccloudregions/2"
         }
         ```

5. **Create a limit object to allow the enterprise to use the datacenter or public cloud region**  
   1. Create a limit object, which represents an allowed location and allocation limits.  
      1. Limits have links to also allow access to resources such as datastoretiers and backup policies, and set default roles.  
   2. Reference: [Limit resource](https://wiki.abiquo.com/api/latest/limit.html)  
   3. To allow unlimited resources in this location, set allocation limit values to 0  
   4. After you allow an enterprise to access a region, you can also set a provider limit. See [https://wiki.abiquo.com/api/latest/EnterpriseLimitsByProviderResource.html](https://wiki.abiquo.com/api/latest/EnterpriseLimitsByProviderResource.html)  
   5. Sample datacenterlimit object

      ```json
      {
        "enabledHardwareProfiles" : false,
        "vmsSoft" : 0,
        "vmsHard" : 0,
        "vlansSoft" : 0,
        "vlansHard" : 0,
        "publicIpsSoft" : 0,
        "publicIpsHard" : 0,
        "links" : [ {
          "rel" : "location",
          "href" : "http://example.com/api/admin/datacenters/1"
        } ],
        "ramSoft" : 0,
        "ramHard" : 0,
        "cpuSoft" : 0,
        "cpuHard" : 0
      }
      ```

6. **Create an enterprise limit to allow an enterprise to use a datacenter or public cloud region**  
   1. Reference: [Create a datacenter limit](https://wiki.abiquo.com/api/latest/EnterpriseLimitsByDatacentersResource.html#create-a-datacenter-limit)  
   2. cURL

      ```bash
      curl -X POST http://example.com/api/admin/enterprises/292/limits \
       -H "Accept: application/vnd.abiquo.limit+json;version=4.7" \
       -H "Content-Type: application/vnd.abiquo.limit+json;version=4.7" \
       -u <USERNAME>:<PASSWORD> \
       -d @datacenterlimit.json
      ```

   3. Sample response.

      ```json
      {  
          "id": 1115,  
          "enabledHardwareProfiles": false,  
          "diskSoftLimitInMb": 0,  
          "diskHardLimitInMb": 0,  
          "storageSoftInMb": 0,  
          "storageHardInMb": 0,  
          "vmsSoft": 0,  
          "vmsHard": 0,  
          "vlansSoft": 0,  
          "vlansHard": 0,  
          "publicIpsSoft": 0,  
          "publicIpsHard": 0,  
          "repositorySoftInMb": 0,  
          "repositoryHardInMb": 0,  
          "links": [  
              {  
                  "title": "enterprise_444",  
                  "rel": "enterprise",  
                  "type": "application/vnd.abiquo.enterprise+json",  
                  "href": "https://example.com:443/api/admin/enterprises/292"  
              },  
              {  
                  "title": "edit datacenter limits 1115",  
                  "rel": "edit",  
                  "type": "application/vnd.abiquo.limit+json",  
                  "href": "https://example.com:443/api/admin/enterprises/292/limits/1115"  
              },  
              {  
                  "title": "external networks",  
                  "rel": "externalnetworks",  
                  "type": "application/vnd.abiquo.vlans+json",  
                  "href": "https://example.com:443/api/admin/enterprises/292/limits/1115/externalnetworks"  
              },  
              {  
                  "title": "virtualappliances",  
                  "rel": "action",  
                  "type": "application/vnd.abiquo.virtualappliances+json",  
                  "href": "https://example.com:443/api/admin/enterprises/292/limits/1115/action/virtualappliances"  
              },  
              {  
                  "title": "Abiquo-DC",  
                  "rel": "location",  
                  "type": "application/vnd.abiquo.datacenter+json",  
                  "href": "https://example.com:443/api/admin/datacenters/1"  
              },  
              {  
                  "title": "NFS Tier",  
                  "rel": "tier",  
                  "type": "application/vnd.abiquo.tier+json",  
                  "href": "https://example.com:443/api/admin/datacenters/1/storage/tiers/1"  
              },  
              {  
                  "title": "Nexenta Tier 2",  
                  "rel": "tier",  
                  "type": "application/vnd.abiquo.tier+json",  
                  "href": "https://example.com:443/api/admin/datacenters/1/storage/tiers/2"  
              },  
              {  
                  "title": "Default Tier 3",  
                  "rel": "tier",  
                  "type": "application/vnd.abiquo.tier+json",  
                  "href": "https://example.com:443/api/admin/datacenters/1/storage/tiers/3"  
              },  
              {  
                  "title": "LVM Tier 4",  
                  "rel": "tier",  
                  "type": "application/vnd.abiquo.tier+json",  
                  "href": "https://example.com:443/api/admin/datacenters/1/storage/tiers/4"  
              },  
              {  
                  "title": "datastoretier_hyperv_2_33",  
                  "rel": "datastoretier",  
                  "type": "application/vnd.abiquo.datastoretier+json",  
                  "href": "https://example.com:443/api/admin/datacenters/1/datastoretiers/40"  
              },  
              {  
                  "title": "datastoretier_DC1_sharedds1_nfs",  
                  "rel": "datastoretier",  
                  "type": "application/vnd.abiquo.datastoretier+json",  
                  "href": "https://example.com:443/api/admin/datacenters/1/datastoretiers/44"  
              },  
              {  
                  "title": "datastoretierESXI6.0_2.29",  
                  "rel": "datastoretier",  
                  "type": "application/vnd.abiquo.datastoretier+json",  
                  "href": "https://example.com:443/api/admin/datacenters/1/datastoretiers/58"  
              },  
              {  
                  "title": "datastoretier_DC1_sharedds2_nfs",  
                  "rel": "datastoretier",  
                  "type": "application/vnd.abiquo.datastoretier+json",  
                  "href": "https://example.com:443/api/admin/datacenters/1/datastoretiers/59"  
              },  
              {  
                  "title": "datastoretier_ESXI_2_56",  
                  "rel": "datastoretier",  
                  "type": "application/vnd.abiquo.datastoretier+json",  
                  "href": "https://example.com:443/api/admin/datacenters/1/datastoretiers/63"  
              },  
              {  
                  "title": "datastoretierESXI6.0_2.23",  
                  "rel": "datastoretier",  
                  "type": "application/vnd.abiquo.datastoretier+json",  
                  "href": "https://example.com:443/api/admin/datacenters/1/datastoretiers/69"  
              },  
              {  
                  "title": "datastoretier_KVM",  
                  "rel": "datastoretier",  
                  "type": "application/vnd.abiquo.datastoretier+json",  
                  "href": "https://example.com:443/api/admin/datacenters/1/datastoretiers/70"  
              },  
              {  
                  "title": "datastoretier_ESXI67_DC1",  
                  "rel": "datastoretier",  
                  "type": "application/vnd.abiquo.datastoretier+json",  
                  "href": "https://example.com:443/api/admin/datacenters/1/datastoretiers/73"  
              },  
              {  
                  "title": "datastoretierESXI6.55555",  
                  "rel": "datastoretier",  
                  "type": "application/vnd.abiquo.datastoretier+json",  
                  "href": "https://example.com:443/api/admin/datacenters/1/datastoretiers/74"  
              },  
              {  
                  "title": "datastoretierESXI5.5_2.51",  
                  "rel": "datastoretier",  
                  "type": "application/vnd.abiquo.datastoretier+json",  
                  "href": "https://example.com:443/api/admin/datacenters/1/datastoretiers/75"  
              },  
              {  
                  "title": "datastoretierESXI_thunder14",  
                  "rel": "datastoretier",  
                  "type": "application/vnd.abiquo.datastoretier+json",  
                  "href": "https://example.com:443/api/admin/datacenters/1/datastoretiers/76"  
              }  
          ],  
          "ramSoft": 0,  
          "ramHard": 0,  
          "cpuSoft": 0,  
          "cpuHard": 0  
      }
      ```

7. **Switch to the new enterprise**  
   1. Reference: [How to switch enterprises via API](#bookmark=id.50l3n18dfd74)  
8. **Create user objects to create users**  
   1. Reference: [User](https://wiki.abiquo.com/api/latest/user.html)  
   2. Get user role and scope links to add to user objects  
   3. For roles, you can use the has` parameter to filter the results with a text string  
      1. IDs of roles and scopes that are useful for testing:  
         * Global scope: 1  
         * Cloud admin: 1  
         * User: 2  
         * Enterprise admin: 3  
   4. For Abiquo authentication, you will need to supply an initial password  
   5. References  
      1. [List roles](https://wiki.abiquo.com/api/latest/RolesResource.html#list-roles)  
      2. [List scopes](https://wiki.abiquo.com/api/latest/ScopesResource.html#list-scopes)  
   6. Example role and scope links  
      1. Role link

         ```json
         {
             "title": "ENTERPRISE_ADMIN_for_training",
             "rel": "edit",
             "type": "application/vnd.abiquo.role+json",
             "href": "https://example.com:443/api/admin/roles/47"
         },
         ```

      2. Scope link

         ```json
         {
             "title": "Reseller Scope",
             "rel": "edit",
             "type": "application/vnd.abiquo.scope+json",
             "href": "https://example.com:443/api/admin/scopes/101"
         },
         ```

   7. Example enterprise admin user

      ```json
      {
                  "nick": "entadmin292",  
                  "name": "Enterprise",  
                  "surname": "Administrator",  
                  "description": "Enterprise admin user",  
                  "email": "entadmin292@example.com",  
                  "locale": "en_US",  
                  "authType": "ABIQUO",  
                  "active": true,  
                  "publicSshKey": "ssh-rsa AAAABblahblahblah",  
                  "allowedCIDRs": [],  
                  "firstLogin": false,  
                  "locked": false,  
                  "phoneNumber": "555123456", 
                  "password": "chgmegih34587YW_pd!",  
                  "links": [  
                      {  
                          "title": "ENTERPRISE_ADMIN",  
                          "rel": "role",  
                          "type": "application/vnd.abiquo.role+json",  
                          "href": "https://example.com:443/api/admin/roles/3"  
                      },  
                      {  
                          "title": "Global scope",  
                          "rel": "scope",  
                          "type": "application/vnd.abiquo.scope+json",  
                          "href": "https://example.com:443/api/admin/scopes/1"  
                      }  
                  ]  
              }
      ```

   8. Example cloud user

      ```json
      {  
          "nick": "clouduser292",  
          "name": "Standard",  
          "surname": "Clouduser",  
          "description": "Standard user",  
          "email": "clouduser292@example.com",  
          "locale": "en_US",  
          "authType": "ABIQUO",  
          "active": true,  
          "allowedCIDRs": [],  
          "firstLogin": false,  
          "locked": false,  
          "password": "chgmeL0_AR7ruhq34kj32423",  
          "links": [  
              {  
                  "title": "USER",  
                  "rel": "role",  
                  "type": "application/vnd.abiquo.role+json",  
                  "href": "https://example.com:443/api/admin/roles/2"  
              },  
              {  
                  "title": "Global scope",  
                  "rel": "scope",  
                  "type": "application/vnd.abiquo.scope+json",  
                  "href": "https://example.com:443/api/admin/scopes/1"  
              }  
          ]  
      }
      ```

9. **Create a user**  
   1. Reference: [Create a user in an enterprise](https://wiki.abiquo.com/api/latest/UsersResource.html#create-a-user-in-an-enterprise)  
   2. cURL:

      ```bash
      curl -X POST https://example.com/api/admin/enterprises/292/users \
       -H "Accept: application/vnd.abiquo.user+json; version=4.7" \
       -H "Content-Type: application/vnd.abiquo.user+json; version=4.7" \
       -u <USERNAME>:<PASSWORD> \
       -d @userentity.json
      ```

   3. Sample response. Success status code: 201

      ```json
      {  
          "id": 231,  
          "nick": "clouduser292",  
          "name": "Standard",  
          "surname": "Clouduser",  
          "description": "Standard user",  
          "email": "clouduser292@example.com",  
          "locale": "en_US",  
          "authType": "ABIQUO",  
          "active": true,  
          "allowedCIDRs": [],  
          "firstLogin": false,  
          "locked": false,  
          "links": [  
              {  
                  "title": "enterprise_444",  
                  "rel": "enterprise",  
                  "type": "application/vnd.abiquo.enterprise+json",  
                  "href": "https://example.com:443/api/admin/enterprises/292"  
              },  
              {  
                  "title": "USER",  
                  "rel": "role",  
                  "type": "application/vnd.abiquo.role+json",  
                  "href": "https://example.com:443/api/admin/roles/2"  
              },  
              {  
                  "title": "clouduser292",  
                  "rel": "edit",  
                  "type": "application/vnd.abiquo.user+json",  
                  "href": "https://example.com:443/api/admin/enterprises/292/users/231"  
              },  
              {  
                  "title": "Global scope",  
                  "rel": "scope",  
                  "type": "application/vnd.abiquo.scope+json",  
                  "href": "https://example.com:443/api/admin/scopes/1"  
              },  
              {  
                  "title": "virtual machines",  
                  "rel": "virtualmachines",  
                  "type": "application/vnd.abiquo.virtualmachines+json",  
                  "href": "https://example.com:443/api/admin/enterprises/292/users/231/action/virtualmachines"  
              },  
              {  
                  "title": "pending tasks",  
                  "rel": "pendingtasks",  
                  "type": "application/vnd.abiquo.tasks+json",  
                  "href": "https://example.com:443/api/admin/enterprises/292/users/231/action/pendingtasks"  
              },  
              {  
                  "title": "applications",  
                  "rel": "applications",  
                  "type": "application/vnd.abiquo.applications+json",  
                  "href": "https://example.com:443/api/admin/enterprises/292/users/231/applications"  
              },  
              {  
                  "title": "enable two factor authentication",  
                  "rel": "enable2fa",  
                  "type": "application/vnd.abiquo.twofactorauthcredentials+json",  
                  "href": "https://example.com:443/api/admin/enterprises/292/users/231/action/enable2fa"  
              },  
              {  
                  "title": "async tasks",  
                  "rel": "asynctasks",  
                  "type": "application/vnd.abiquo.asynctasks+json",  
                  "href": "https://example.com:443/api/admin/enterprises/292/users/231/asynctasks"  
              }  
          ]  
      }
      ```

10. **Add templates to the enterprise's Apps library.** See [How to add a VM template in a datacenter via API](#bookmark=id.2aqo3qhrhj2k)