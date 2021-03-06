# ago-tools

A Python package to assist with administering ArcGIS Online Organizations.

## Features
* Create a spreadsheet of all users in the org
* Update map service urls in webmaps
* Search for new users and add them to a list of groups
* Move (migrate) all items between accounts (single or batch)

## Instructions

1. Fork and then clone the repo. 
2. Run and try the samples.

## Installation
Unzip into a folder such as C:/myscripts and remove dashes from the directory name.
e.g. `C:/myscripts/ago-tools-master` to `C:/myscripts/agoTools`

Then do one of the following:

* add that directory to your system path in advanced system settings
* append it at runtime using the sys module in python
    
        import sys
        sys.path.append('c:/myscripts')

## Samples

### Admin Class
 
#### Create a spreadsheet of all users in the org
    # Requires admin role.
    import csv, time
    from agoTools.admin import Admin
	
    agoAdmin = Admin(<username>) # Replace <username> with your admin username.
    users = agoAdmin.getUsers()

    outputFile = 'c:/temp/users.csv'

    with open(outputFile, 'wb') as output:
        dataWriter = csv.writer(output, delimiter=',', quotechar='|', quoting=csv.QUOTE_MINIMAL)
        # Write header row.
        dataWriter.writerow(['Full Name', 'Email', 'Username', 'Role', 'Date Created'])
        # Write user data.
        for user in users:
            dataWriter.writerow([user['fullName'], user['email'], user['username'], user['role'], time.strftime("%Y-%m-%d",time.gmtime(user['created']/1000))])

#### Add new users to existing groups
    # Requires admin role.
    import csv, time, datetime
    from agoTools.admin import Admin

    # User parameters:
    agoAdmin = Admin(<username>)   # Replace <username> with your admin username.
    daysToCheck = 7   # Replace with number of days to check...1 checks past day, 7 checks past week, etc.
    groups = [<groupID1>, <groupID2>, ...]   # Enter <groupIDs> of groups to which you want to add new users
    # Find the group ID with this tool: http://developers.arcgis.com/en/javascript/samples/portal_getgroupamd/
    outputDir = 'c:/temp/'   # Replace with path for report file
	
    outputDate = datetime.datetime.now().strftime("%Y%m%d")   # Current date prefixed to filename.
    outputFile = outputDir + outputDate + '_AddNewUsers2Groups.csv'
	
    newUsers = agoAdmin.getNewUsers(daysToCheck)
    userSummary = agoAdmin.addUsersToGroups(newUsers, groups)
    # print userSummary # Uncomment this line to see a summary of the group additions.
    # Reports false-negatives as of Nov 5, 2013.
	
    with open(outputFile, 'wb') as output:
        dataWriter = csv.writer(output, delimiter=',', quotechar='|', quoting=csv.QUOTE_MINIMAL)
        # Write header row.
        dataWriter.writerow(['Full Name', 'Email', 'Username', 'Role', 'Date Created'])
        # Write user data.
        for user in newUsers:
            dataWriter.writerow([user['fullName'], user['email'], user['username'], user['role'], time.strftime("%Y-%m-%d", time.gmtime(user['created']/1000))])

#### Move all items from one account to another, reassign ownership of all groups, or add user to another user's groups
    # Requires admin role
    # If you want to do all three tasks at once, see migrateAccount or migrateAccounts functions
	
    from agoTools.admin import Admin
    agoAdmin = Admin(<username>)  # Replace <username> with your admin username
    
    agoAdmin.reassignAllUser1ItemsToUser2(agoAdmin, <userFrom>, <userTo>)  #Replace with your current and new account usernames
    agoAdmin.reassignAllGroupOwnership(agoAdmin, <userFrom>, <userTo>)
    agoAdmin.addUser2ToAllUser1Groups(agoAdmin, <userFrom>, <userTo>)
    
#### Migrate person to a new account within the same Org
    # Requires admin role
    # Useful when migrating to Enterprise Logins.
    # Reassigns all items/groups to new owner and
    # adds userTo to all groups which userFrom is a member.'''

    from agoTools.admin import Admin
	
    myAgol = Admin('<username>')  # Replace <username> your ADMIN account
    
    # for migrating a single account...
    myAgol.migrateAccount(myAgol, '<userFrom>', '<userTo>')   # Replace with usernames between which you are moving items
    # for migrating a batch of accounts
    myAgol.migrateAccounts(myAgol, <path to user mapping CSV>)   # Replace with path to CSV file with col1=userFrom, col2=userTo
  
### Utilities Classs
            
#### Update map service urls in webmaps
    from agoTools.utilities import Utilities
    agoUtilities = Utilities(<username>) # Replace <username> with your username.

    webmapId = 'e1d78110b0eg447aab46d373c7360046'
    oldUrl = 'http://myserver.com/arcgis/rest/services/old/MapServer'
    newUrl = 'http://myserver.com/arcgis/rest/services/new/MapServer'

    agoUtilities.updateWebmapService(webmapId, oldUrl, newUrl)
    
#### Update the URL for registered map services or web applications
    from agoTools.utilities import Utilities
    agoUtilities = Utilities(<username>) # Replace <username> with your username.

    itemId = 'e1d78110b0eg447aab46d373c7360046'
    oldUrl = 'http://oldserver.com/app'
    newUrl = 'http://newserver.com/app'

    agoUtilities.updateItemUrl(itemId, oldUrl, newUrl)	

## Requirements

* Python
* Notepad or your favorite Python editor

## Resources

* [Python for ArcGIS Resource Center](http://resources.arcgis.com/en/communities/python/)
* [ArcGIS Blog](http://blogs.esri.com/esri/arcgis/)
* [twitter@esri](http://twitter.com/esri)

## Issues

Find a bug or want to request a new feature?  Please let us know by submitting an issue.

## Contributing

Esri welcomes contributions from anyone and everyone. Please see our [guidelines for contributing](https://github.com/esri/contributing).

## Licensing
Copyright 2013 Esri

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

A copy of the license is available in the repository's [license.txt](https://raw.github.com/Esri/ago-tools/master/license.txt) file.

[](Esri Tags: ArcGIS-Online Python Tools Library)
[](Esri Language: Python)
