# RandomKit reproducible sampling example

Creates an explicitly seeded generator, draws integers and a Double, shuffles cards, samples without replacement, and clones the generator state.

From the repository root, create an isolated consumer using the existing sealed
package (building directly in this checkout discovers unrelated app resources):

```sh
example_dir=$(mktemp -d)
mkdir "$example_dir/packages"
cp examples/randomkit/main.bui "$example_dir/main.bui"
cp packages/randomkit-0.1.0.buipkg "$example_dir/packages/"
(cd "$example_dir" && buoy main.bui --check --diagnostic-format json && buoy main.bui -o app && ./app)
```

For a standalone consumer, copy `main.bui` into a fresh directory and place
`randomkit-0.1.0.buipkg` from the repository's `packages/` in that directory's
`packages/`. Run `buoy main.bui -o app` and `./app` there. No native libraries
or services are required.

The first word is `3499211612`, the hand has two cards, and the final comparison is true. Shuffle and sampling consume draws; changing their order changes subsequent output. This generator is for reproducible simulations, not security.
