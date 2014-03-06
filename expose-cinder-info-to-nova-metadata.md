# Exposing Cinder information to Nova Metadata Service

Currently the Nova Metadata service exposes various data to tenant instances over HTTP.

We would like to expose the mapping of Cinder devices to instance in the Nova Meta data API.  This be used by Ironic Guest instances to mount a remote iSCSI volume themselves.

In the Icehouse cycle, support for exposing data and information from Neutron was added:
  * [Blueprint](https://blueprints.launchpad.net/nova/+spec/metadata-service-network-info)
  * [Code Review](https://review.openstack.org/#/c/74002/)

Following the pattern of the Neutron/Network additions, for `JUNO` or later versions of the meta data API, an additional key `volume_data` would be added.  The value of this key would contain a specification of a Volume name, Volume Size, URL, and any other information necessary for a tenant instance to mount it directly.  This value should correspond directly to the value returned by Cinder's Detailed response view.  An operator may chose to filter some information, for example in the case of a non-Ironic or virtualized instance, the information used to mount it, such as internal IPs or URLs may be committed.

An example response:

```javascript
{
	// other metadata fields
	'volume_data': [
		{
			'id': '1234'
			'name': 'test volume',
			'status': 'ACTIVE',
			'size': 4000,
			'url': 'iscsi://.....????',
		}
	]
}

```