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

Each Shard

shard_tenant
shard_tenant_snapshot
shard_tenant_setting
shard_tenant_setting_snapshot
shard_tenant_user
shard_tenant_user_snapshot
shard_release_configuration
shard_release_configuration_snapshot

Example module tables

crm_customer - aggregate
crm_customer_address - entity (belongs to customer)
crm_address - aggregate

cms_document - aggregate
cms_document_files - entity

For moving/replicating, each module needs to,
- perform writes to all shards
- move records by UUID (shard ID may differ between shards)
