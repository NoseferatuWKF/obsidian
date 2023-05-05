# [Casbin](https://casbin.org/docs/overview)

>Casbin is an authorization library which can be used in flows where we want a certain object or entity to be accessed by a specific user or subject. The type of access i.e. action can be read, write, delete or any other action as set by the developer. This is how Casbin is most widely used and its called the "standard" or classic { subject, object, action } flow.

>Casbin is capable of handling many complex authorization scenarios other than the standard flow. There can be addition of roles (RBAC), attributes (ABAC) etc.

## What Casbin does:

1. Enforce the policy in the classic { subject, object, action } form or a customized form as you defined. Both allow and deny authorizations are supported.
2. Handle the storage of the access control model and its policy.
3. Manage the role-user mappings and role-role mappings (aka role hierarchy in RBAC).
4. Support built-in superusers like root or administrator. A superuser can do anything without explicit permissions.
5. Multiple built-in operators to support the rule matching. For example, keyMatch can map a resource key /foo/bar to the pattern /foo*.

## What Casbin does NOT do:

1. Authentication (aka verify username and password when a user logs in)
2. Manage the list of users or roles.

## How it Works

>In Casbin, an access control model is abstracted into a CONF file based on the PERM metamodel (Policy, Effect, Request, Matchers). So switching or upgrading the authorization mechanism for a project is just as simple as modifying a configuration. You can customize your own access control model by combining the available models. For example, you can combine RBAC roles and ABAC attributes together inside one model and share one set of policy rules.

>The PERM model is composed of four foundations (Policy, Effect, Request, Matchers) describing the relationship between resources and users.

model.conf
```bash
# Request definition
[request_definition]
r = sub, obj, act

# Policy definition
[policy_definition]
p = sub, obj, act

# Policy effect
[policy_effect]
e = some(where (p.eft == allow))

# Matchers
[matchers]
m = r.sub == p.sub && r.obj == p.obj && r.act == p.act
```

example policy for ACL models
```
p, alice, data1, read # alice can read data1
p, bob, data2, write # bob can write data2
```

## [Enforcers](https://casbin.org/docs/enforcers)

>Enforcer is the main structure in Casbin. It acts as an interface for users to make operations on policy rules and models.

## [Adapters](https://casbin.org/docs/adapters)

>In Casbin, the policy storage is implemented as an adapter (aka middleware for Casbin). A Casbin user can use an adapter to load policy rules from a storage (aka LoadPolicy()), or save policy rules to it (aka SavePolicy()).

1. If casbin.NewEnforcer() is called with an explicit or implicit adapter, the policy will be loaded automatically.
2. You can call e.LoadPolicy() to reload the policy rules from the storage.
3. If the adapter does not support the Auto-Save feature, The policy rules cannot be automatically saved back to the storage when you add or remove policies. You have to call SavePolicy() manually to save all policy rules.

## [Watchers](https://casbin.org/docs/watchers)

## [Dispatchers](https://casbin.org/docs/dispatchers)