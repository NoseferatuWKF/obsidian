## Installation

```bash
# no hassle installation
sudo apt install -y default-jre default-jdk
```

## Basics

```java
class Main {
	private static class WhyIsThisAllowed {
		long id;
		String name;

		// ctor
		WhyIsThisAllowed(long id, String name) {
			this.id = id;
			this.name = name;
		}

		private void printName() {
			System.out.println(this.name);
		}
	}

	// The bane of programming
	public static void main(String[] args) {
		// Once inside here only static members are allowed
		System.out.println("Java sucks btw");

		WhyIsThisAllowed iDunnoWhy = new WhyIsThisAllowed(0, "magic");
		// this was supposed to be a private method
		iDunnoWhy.printName();
	}

}
```

iterator loop

```java
// let's say we have an array of strings elems
for (String el: elems) {
	System.out.println(el);
}
```

compiling and running

```shell
javac <filename>.java
# arguments can be passed here once compiled
java <filename> # the compiled bytecode .Class file
```

autoboxing
https://javarevisited.blogspot.com/2010/10/what-is-problem-while-using-in.html#axzz82BXif5vU

reflection

threadpool

packages

## Integration

### nvim

jdtls
```bash
# get java 20 (at least 17 for lsp) 
curl -LO https://download.oracle.com/java/20/latest/jdk-20_linux-x64_bin.deb
# for non debian
curl -LO https://download.oracle.com/java/20/latest/jdk-20_linux-x64_bin.tar.gz 
```

mason
```lua
lspconfig.jdtls.setup {
	capabilities = capabilities,
	on_attach = on_attach,
	settings = servers[server_name],
	root_dir = lspconfig.util.root_pattern('.git')
}
```
