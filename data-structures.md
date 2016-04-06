How to create a socket for an address family

```c
include/linux/net.h
struct net_proto_family {
  int family;  // AF_INET, AF_UNIX, AF_INET6, etc. must: 0 <= family < NPROTO
  int (*create)(struct net *net, struct socket *sock, int protocol, int kern);
  struct module *owner;
};

```


```c
// net/socket.c
static const struct net_proto_family *net_families[NPROTO];

// net/ipv4/af_inet.c
static int inet_create(struct net *net, struct socket *sock, int protocol, int kern);

static const struct net_proto_family inet_family_ops = {
  .family = PF_INET,
  .create = inet_create,
  .owner  = THIS_MODULE
};

// inet_init() calls sock_register():
net_families[AF_INET] = &inet_family_ops;
```

```
int sys_socket(int family, int type, int protocol)
  -> struct socket* sock_create(family, type, protocol)
    -> __sock_create(family, type, protocol)
      -> struct socket* sock = sock_alloc()
        -> new_inode_pseudo(super_block of sockfs)      // fs/inode.c
      -> net_families[family]->create(sock, protocol)
        -> inet_create(sock, protocol)                  // net/ipv4/af_inet.c
```
