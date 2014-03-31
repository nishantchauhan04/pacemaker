# Updating schema files #

## Experimental features ##

Experimental features go into `${base}-next.rng`

Create from the most recent `${base}-${X}.${Y}.rng` if it does not already exist

## Stable features ##

The current stable version is determined from the `known_schemas` array in `lib/common/xml.c`.
It will be the entry prior to `pacemaker-next` and has the form `pacemaker-${X}.${Y}`.

### Simple Additions

When the new syntax is a simple addition to the previous one, create a new entry with `${Y} = ${Yold} + 1` 

### Feature Removal or Incompatible Changes

When the new syntax is not a simple addition to the previous one, create a new entry with `${X} = ${Xold} + 1` and `${Y} = 0`.
An XSLT file is also required that converts an old syntax to the new one.
See `xml/upgrade06.xsl` for an example.


Bump `${X}` and set `${Y}` to `0` for syntax removals, bump `${Y}` for additions and then,

### General Proceedure

1. Add the new `${X}.${Y}` to `RNG_versions` in xml/Makefile.am,
1. Copy the most recent version of `${base}-*.rng` to `${base}-${X}.${Y}.rng`
1. Update `known_schemas` in `lib/common/xml.c`, add it prior to the entry for `pacemaker-next`.
   The second last field of the previous entry needs to be set to `0`.
   For example:
            
        /* 6 */    { 2, "pacemaker-1.2",  NULL, NULL, -1, NULL },
        /* - */    { 2, "pacemaker-next", NULL, NULL, -1, NULL }, /* Feature playground */
            
   becomes:
            
        /* 6 */    { 2, "pacemaker-1.2",  NULL, NULL,  0, NULL },
        /* 7 */    { 2, "pacemaker-1.3",  NULL, NULL, -1, NULL },
        /* - */    { 2, "pacemaker-next", NULL, NULL, -1, NULL }, /* Feature playground */


## Admin Tasks
New features will not be available until the admin

1. Updates all the nodes
1. Runs the equivalent of cibadmin --upgrade