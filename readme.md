This config format is fully compatible with nginx config. 

## Directive

```
<directive> <parameters>; 
```

nc directives

```nginx
param_string value;
param_integer 100;
param_float 1.2;
param_duration 5m;
param_units 10k;
param_flag on;

array_values one two three;
map_values key1 value1;
map_values key2 value2;
map_inline_values key1=value1 key2=value2;
```
equals to JSON
```json
{
  "param_string": "value",
  "param_integer": 100,
  "param_float": 1.2,
  "param_duration": 13500,
  "param_units": 10000,
  "param_flag": true,
  
  "array_values": ["one", "two", "three"],
  "map_values": {
    "key1": "value1",
    "key2": "value2"
  },
  "map_inline_values": {
    "key1": "value1",
    "key2": "value2"
  }
}
```

## Sections

```
<section> {
  <directive> <parameters>; 
}
# or
<section> <key> {
  <directive> <parameters>; 
}
```

```nginx
section {
  numbers 4 4ki 16k;
}

array_section {
  param1 one;
}
array_section {
  param2 two;
}
array_section {
  param3 three;
}

mapped_section one {
  param4 4;
}
mapped_section two {
  param5 8;
}
mapped_section three {
  param6 16;
}

nested {
  section {
    param7 off;
  }
}
```
equals to JSON
```json
{
  "section": {
    "numbers": [4, 4096, 16000]
  },
  
  "array_section": [
    {
      "param1": "one"
    },
    {
      "param2": "two"
    },
    {
      "param3": "three"
    }
  ],
  
  "mapped_section": {
    "one": {
      "param4": 4
    },
    "two": {
      "param5": 8
    },
    "three": {
      "param6": 16
    },
  },
  
  "nested": {
    "section": {
      "param7": false
    }
  }
}
```

## Variables

Variables starts with `$` symbol (`$user`, `$root`) and can be used anywhere in config:

```nginx
path $root;
logs $root/logs;
```

To set variable use `set` directive:

```nginx
set $logs $root/logs;

access_log $logs/access.log;
audit_log $logs/audit.log;
error_log $logs/error.log;
```

## Include

Directive `include <file>` import directives from file to the current/global section

`extra.inc`:
```nginx
param_x 10;
param_y 8;
param_z -1;
```

```nginx
root /www;
include extra.inc;

section {
  include extra.inc;
  action none;
}
```
equals to 
```nginx
root /www
param_x 10;
param_y 8;
param_z -1;

section {
  param_x 10;
  param_y 8;
  param_z -1;
  action none;
}

```

