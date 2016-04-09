# Reassign

<img src="https://raw.githubusercontent.com/psynteract/psynteract-os/master/plugins/psynteract_reassign/psynteract_reassign_large.png" align="right">

Psynteract automatically assigns groups when the session is started. In case these
groups should be reassigned during the experiment, psynteract provides the option to
*reassign* clients during the experiment. As psynteract prepares all groupings
in advance when starting the session, the *total number of groupings needed* has to be
provided explicitly when [connecting](usage-connect.md#optional) to the session.

## OpenSesame

To reassign clients at a specific point in the experiment, include the *reassign*
item in the experiment. This will assign the client to the next grouping.

If more assignments are made than the number of groupings specified in the *connect*
item, this will cause an error. To prevent this, the option *allow rollover in case
the maximum number of groupings is exceeded* can be used. This might be helpful, for
example, when the reassign item is nested in a loop structure (which will by default
lead to one more reassignment than necessary, see [prisoner's dilemma example experiment] (https://github.com/psynteract/psynteract-os/tree/master/examples)).

## Python

A connection's **`reassign_grouping`** method can be used to reassign clients and
update all variables related to the current grouping.

```python
c.reassign_grouping(allow_rollover=True)
```

### Parameters

The **`allow_rollover`** argument specifies if rollovers are permitted, if more (re)assignments
are made than the number groupings that was prepared in advance. If it is `False` (the default),
an error will occur in this case. If it is `True`, psynteract will restart from the first grouping
instead.
