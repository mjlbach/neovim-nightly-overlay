# To get nix and set up the binary cache

Follow the instructions [here](https://app.cachix.org/cache/mjlbach) to set up nix and add my cachix cache which provides precompiled binaries, built against the nixpkgs-unstable channel each night.

# To use the overlay 

Add the following to your $HOME/.config/nixpkgs/overlays directory: (make a file $HOME/.config/nixpkgs/overlays/neovim.nix and paste the snippet below into that file)

```nix
import (builtins.fetchTarball {
      url = https://github.com/mjlbach/neovim-nightly-overlay/archive/master.tar.gz;
    })
```

If usings flakes, add to your nixpkgs overlays attribute (examples will differ, the following is for home-manager):
```nix
{
  inputs.neovim-nightly-overlay.url = "github:mjlbach/neovim-nightly-overlay/flakes";
  outputs = { self, ... }@inputs:
    let
      overlays = [
          inputs.neovim-nightly-overlay.overlay
        ];
    in
      homeConfigurations = {
        macbook-pro = inputs.home-manager.lib.homeManagerConfiguration {
          configuration = { pkgs, ... }:
            {
              nixpkgs.overlays = overlays;
            };
        };
      };
}
```

Note, I recently switched the overlay to use flakes by default with flakes-compat for older nix. Please report issues if this breaks things.

Install neovim-nightly:
```
nix-env -iA pkgs.neovim-nightly
```
or add to home-manager/configuration.nix.

