---
title: Dynamics CRM User and Roles report with DynamicsNode
tags:
---
This piece of code creates a pivot table with the users and the roles assigned to it in Dynamics CRM



``` js
// Exports the users under the specified BU and the roles that each one has
var BUName = 'Brokerage';
// set the environment where to execute this script
var env = 'PROD';

// dependencies
var dn = require('dynamicsnode');
var fs = require('fs');

var crm = new dn.CRMClient(env); 

// Get the BU and Sub BU's
console.log('Getting BUs....');
function getSubBUs(parentBUId,level){
    level = level || '-';
    var subBUs=[];
    var childrendBUs = crm.retrieveMultiple('businessunit',{parentbusinessunitid:parentBUId},['businessunitid','name']);
    for (var i = 0; i < childrendBUs.rows.length; i++) {
        var childrenBU = childrendBUs.rows[i];
        console.log(level + childrenBU.name);
        subBUs.push(childrenBU.businessunitid);
        subBUs = subBUs.concat(getSubBUs(childrenBU.businessunitid,level+'-'));
    }
    return subBUs;
}

var parentBU = crm.retrieve('businessunit',{name:BUName},'businessunitid');
var BUs = [parentBU.businessunitid];
BUs = BUs.concat(getSubBUs(parentBU.businessunitid));

var report = new dn.DataTable('UserRolesReport');

// Get the users under the found BUs
var users = crm.retrieveMultiple('systemuser',{businessunitid:BUs},['systemuserid','fullname','domainname','businessunitid','isdisabled']);

// add the users to the report
console.log('Reading users and roles...');
for (var i = 0; i < users.rows.length; i++) {
    var user = users.rows[i];

    var report1Item = {_userFullName:user.fullname, _userDomainName:user.domainname,
                        _buName:user.businessunitid_name,_buId:user.businessunitid,_userIsDisabled:user.isdisabled};

    // get the roles of each user
    var userRoles = crm.retrieveMultiple('systemuserroles',{systemuserid:user.systemuserid},['roleid']);

    for (var j = 0; j < userRoles.rows.length; j++) {
        var userRole = userRoles.rows[j];
        var role = crm.retrieve('role',userRole.roleid,'name');
        var roleName = role.name;//.replace(/\s/g,'_'); // remove spaces
        
        report1Item[roleName]='Y';
    }

    report.rows.push(report1Item);
}

// save the report
dn.DataTableSerializer.save(report,`UserRolesReport-${BUName}-${env}.xml`);

console.log('done!');
```