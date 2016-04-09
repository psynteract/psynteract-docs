# Communicate (OpenSesame only)

<img src="https://raw.githubusercontent.com/psynteract/psynteract-os/master/plugins/psynteract_communicate/psynteract_communicate_large.png" align="right">

The *communicate* item in OpenSesame is a sequential combination of the *push*,
*wait*, and *get* items. This reflects the fact that these operations are
frequently used in combination. The communicate item performs three operations:

1. It sends data from the client to the server
  (see [push item](usage-push.md#opensesame)).
2. It waits until all clients have sent their data
  (see [wait item](usage-wait.md#opensesame)).
3. It retrieves the data provided by the other partners
  (see [get item](usage-get.md#opensesame)).

The options of the communicate item are described in the documentation of the
 *push*, *wait*, and *get* items (as linked above).
