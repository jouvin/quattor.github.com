---
layout: documentation
title: pam
category: documentation
subcategory: components
---
### NAME

NCM::pam - NCM pam configuration component

### SYNOPSIS

    ### declare what pam modules are available.
    "/software/components/pam/modules" = npush("krb5", 
	nlist("path", "/lib/security/$ISA/pam_krb5.so"));
    "/software/components/pam/modules" = npush("cracklib", 
	nlist("path", "/lib/security/$ISA/pam_cracklib.so"));

    ### declare what services we expect to already exist
    "/software/components/pam/services/system-auth" = nlist("predefined", true);

    ### declare an ACL
    "/software/components/pam/acls" = npush("ssh_allow", nlist(
		"items", list("bob", "fred"),
		"file", "/etc/filelist",
     ));

    ### setup a service
    final "/software/components/pam/services/ssh" = nlist(
		"password", list(
			pam_include("system-auth"),
			nlist("required", pam_module_unix()),
			nlist("requisite", pam_module_env()),
		),
		"auth", list(
			pam_include("system-auth"),
			nlist("required", pam_module_if("uid >= 500")),
			nlist("required", pam_acl_allow("ssh_allow")),
		),
	),
     );

- Configure()

    Returns
    error in case of failure.

- Unconfigure()

    not available.

### RESOURCES

- /software/components/pam/active : boolean

    activates/deactivates the component.

- /software/components/pam/modules : nlist

    contains the list of supported PAM modules. For each module, the value
    should be an nlist containing that path for that named module. The
    name can be anything you want. When describing the PAM configuration
    for services, the names provided here are the only acceptable names
    of modules, so it is the responsibility of the operating system
    templates to define the available modules.

- /software/components/pam/services: nlist

    contains an nlist of services that are being controlled by this
    component. Any service controlled will have it's PAM configuration
    completely replaced by this module. Stacked or included PAM 
    configurations
    (i.e. configurations that use pam\_stack.so or the include directive) 
    must ensure that the
    service being stacked is already defined. 
    For each service, the value
    will be an nlist keyed off the module type (auth, account, session,
    password). The value for each module type is an ordered list of
    mappings. Each mapping is an nlist that is keyed off the action (i.e. required, optional, etc),.
    Only a single action is expected in each nlist. 

    The nlist may contain the key "predefined" with a boolean value. If set
    to true, then this service name is expected to be on the system, but will not
    be actively managed by this component. 
    For example, the "system-auth"
    service can be listed with this value, which will allow other services 
    to stack/include
    that service configuration, without requiring that this component 
    take over management of the "system-auth" component.

    PAM config files for 
    services which are not 
    specified within this list will not be touched.

- /software/components/pam/acls: nlist

    Every ACL being managed by this component must be given a name
    and placed into this nlist. The value of the ACL is itself an nlist containing:

    - items

        a list of items to place into the acl

    - file

        optionally the filename for the ACL. If this is not provided, then a filename
        will be generated based on /software/components/pam/acldir, the name of
        the ACL and the sense in which it is being used.

- /software/components/pam/directory: string

    The directory where the config files will be placed, defaulting to /etc/pam.d

- /software/components/pam/acldir: string

    The directory where the ACL files will be placed, defaulting to /etc/pam.acls

### FUNCTIONS

The component provides the following functions to assist in creating configurations:

- pam\_include(SERVICENAME)

    generates configuration which either stacks or includes the
    named service. The named service must be already defined.

- pam\_module\_\*(OPTIONS)

    generates config for using the named pam module. If you wish, you
    can also just use pam\_module(MODULE\_NAME, OPTIONS). The named
    functions are there to provide extra validation and readability.

- pam\_acl\_allow(ACLNAME)

    generates config for using the "filelist" pam module in 'allow'
    mode, based on the provided ACL. The argument should be the name
    of an ACL listed in /software/components/pam/acls. This requires
    the "filelist" module to have been previously defined within
    /software/components/pam/modules.

- pam\_acl\_deny(ACLNAME)

    generates config for using the "filelist" pam module in 'deny'
    mode, based on the provided ACL. The argument should be the name
    of an ACL listed in /software/components/pam/acls. This requires
    the "filelist" module to have been previously defined within
    /software/components/pam/modules.

- pam\_acl\_access(ACLNAME)

    generates config for using the "access" pam module. The argument
    should be the name of an ACL listed in /softweare/components/pam/acls.
    This requires the "access" module to have been previously defined
    within /software/components/pam/modules.

- XXX
- pam\_add(SERVICE, TYPE, CONTROL, MODULE, OPTIONS?)

    This function should be applied to
    "/software/components/pam/services". A mapping is appended to the
    appropriate point in the configuration tree. SERVICE refers to the
    service name being configured (e.g. "sshd"). TYPE refers to the module
    type (e.g. "auth"). CONTROL refers to the action that is taken when
    the PAM system encounters this mapping (e.g. "required"). MODULE is
    the name of a module listed within
    "/software/components/pam/modules". OPTIONS is an optional argument;
    if supplied it should be an nlist containing additional information
    for the PAM module.

- pam\_add\_stack(SERVICE, TYPE, CONTROL, STACKEDSERVICE)

    This function should be applied to
    "/software/components/pam/services". A mapping is appended to the
    appropriate point in the configuration tree. SERVICE refers to the
    service being configured (e.g. "sshd").  TYPE refers to the module
    type (e.g. "auth"). CONTROL refers to the action that is taken when
    the PAM system encounters this mapping
    (e.g. "required"). STACKEDSERVICE is the name of the service that
    should be stacked (e.g. "system-auth").

- pam\_add\_acl(SERVICE, TYPE, CONTROL, SENSE, ITEMTYPE, ITEMS, ONERR?)

    This function should be applied to
    "/software/components/pam/services". A mapping is appended to the
    appropriate point in the configuration tree. SERVICE refers to the
    service being configured (e.g. "sshd").  TYPE refers to the module
    type (e.g. "auth"). CONTROL refers to the action that is taken when
    the PAM system encounters this mapping (e.g. "required"). SENSE is how
    the ACL should be interpreted (e.g. "allow" or "deny"). ITEMTYPE
    declares what the types of items are that are checked
    (e.g. "user"). ITEMS is a list of strings: the content of the
    ACL. Finally, the ONERR parameter (if specified) says how the PAM
    system should consider an internal error (e.g. if the ACL file is
    unreadable).

    This uses the "filelist" module (so that module must be pre-declared
    as existing within "/software/components/pam/modules") and the
    component will construct ACL files in /etc/pam.acls.

    ### FILES MODIFIED

    The component pam modifies files within the /etc/pam.d directory the /etc/pam.acls directory.

    - none.

    ### DEPENDENCIES

    #### Components to be run before:

    none.

    #### Components to be run after:

    none.

    ### EXAMPLES

    "/software/components/pam/active" = true;

    ### BUGS

    none known.

    ### SEE ALSO

    ncm-ncd(1)
