# ls-module-flatfoot
LemonStand module that synchronizes database CMS objects (templates, partials, and pages) with filesystem CMS objects.

## Installation
1. Download FlatFoot
1. Create a folder named `flatfoot` in the `modules` directory.
1. Extract all files into the `modules/flatfoot` directory (`modules/flatfoot/README.md` should exist).
1. Done!

## Usage
While logged into the Administration Area, each page reload (either in the backend or frontend) will synchronize any changes from the database into the filesystem, or any changes from the filesystem into the database.  
  
You can locate the filesystem CMS objects at `resources/cms` in `resources/cms/templates`, `resources/cms/partials`, `resources/cms/pages`.   
  
### Creation/Deletion
In order to add a CMS object from the filesystem, simply create them as you normally would but with sanitized filenames in mind (colons become semicolons, read below). However, in order for FlatFile to update filesystem files from changes made in the Administration Area, they must belong to the same user/group as PHP.  
  
In order to delete a CMS object from the filesystem, simply save the file with no content, or just `-` as the content. Again, correct file permissions must be set.

### Enable Debugging
In order to see what FlatFoot is doing upon page refresh, simply enable `DEV_MODE` in `config/config.php`. A log will be places in `logs/flatfoot.txt` (assuming LS has write permissions).

## Important
FlatFoot relies on file modification dates; therefore your OS time should be the same as your LemonStand configuration (which can be found in `config/config.php`).

## Technical
FlatFoot iterates the database templates/partials/pages, it adds any that don't exist on the filesystem, if it does exist then it compares the modified date to decide which to copy to the other.  
  
FlatFoot is run under the PHP process, and therefore creates files as `www-data` or other. If you copy the module to a different server, the files should have the same user/group as PHP, or else a `chown www-data:www-data -R ./` may be in order.  
  
FlatFoot automatically creates directories that don't exist, and does a `chmod -R 0777 ./` for easy editing. You may want to remove the filesystem CMS objects when you are finished editing.  
  
FlatFoot uses JSON to encode CMS object definitions, which is currently not *'tidy.'*  

FlatFoot must replace characters which are reserved on some operating systems (mostly Windows). For partials it replaces colons with semi-colons, for pages it uses the page path and replaces non-latin characters with underscores.  

## Credit
Prior to creating FlatFoot, I attempted modifying [Yellow Canvas by Panthr](http://forum.lemonstandapp.com/topic/991/lemonstand-module-yellow-canvas/), but it solves problems in a different fashion than for my needs.

## License
`ls-module-flatfoot` is released under the MIT license. A copy of the MIT license can be found in the `LICENSE` file.
