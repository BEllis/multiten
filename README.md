Project Aims
------------

Build a framework for a sharded multi-tenancy application to which modules can be added.

- Support for complex multi-tenancy hierarchies (multiple parents / children).
- Support for sharded multi-tenancy data stores.
- Support for moving / replicating multi-tenancy data stores.
- Support for making changes in "Release Configuration"s (A grouping of changes to take affect at the same time).

Each "aggregate" has the following common characteristics.

aggregateId - Shard Local ID (Used for references to/from entities inside the aggregate / shard?)
aggregateUuid - Universally Unique ID (Used for references between aggregates)
tenantId - The tenant that owners the entity

PK = aggregateId
INDEX = aggregateUuid
FOREIGN KEY tenantId -- Remove for performance gains?

Each "aggregate snapshot" has the following common characteristics.

aggregateId - Shard Local ID (Used for references to/from entities inside the aggregate / shard?)
sequence - sequence number
userId - The identity of the user that made this change.
createdDateTime - The time the change was added.
releaseConfigId - The release configuration to which this version was added.

UNIQUE (aggregateId, releaseId)

Each Shard (Backend)

shard_tenant
shard_tenant_snapshot
shard_tenant_log
shard_data
shard_tenant_setting
shard_tenant_setting_snapshot
shard_security_user
shard_security_user_snapshot
shard_release_configuration
shard_release_configuration_log
shard_release_configuration_changes (CUD + type + uuid + sequence)

(frontend)
frontend_releases
release_xyz_info
release_xyz_diff
release_xyz_paths
release_xyz_rules
release_xyz_views

Example module tables

crm_customer - aggregate
crm_customer_address - entity (belongs to customer)
crm_address - aggregate

cms_document - aggregate
cms_document_files - entity

For moving/replicating, each module needs to,
- perform writes to all shards
- move records by UUID (shard ID may differ between shards)

Git support

cms.xml
- links files with cms documents
cms-cli push <release config>
- pushes cms.xml changes to specified release condiguration 
cms-cli watch <release config>
- watches for local file changes and pushes to CMS (and force preview refresh if enabled)

Use cases
-----------------

As an account user administrator,
I want to add a basic auth login (of type wz-login)
So that my team members can log in to help manage my sites.

As an account user administrator,
I want to create an access user on my account (or one of my child accounts)
So that after my team members log in they have access to one or more accounts.

Criteria: access user must be linked to at least one login.

