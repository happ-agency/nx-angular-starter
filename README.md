# [Nx](https://nx.dev/) [Angular](https://angular.io/) Starter

> Based on the [happ/nx-starter](https://github.com/happ-agency/nx-starter)

1. To install [Angular](https://angular.io/) run: `pnpm i -D @nrwl/angular`
2. To create an [Angular](https://angular.io/) `app` run: `npx nx g @nrwl/angular:app APP_NAME --style=scss --routing=true`
	 > 1. `APP_NAME` - name of the application
	 > 2. `--style=scss` - default styles
	 > 3. `--routing=true` - use routes by default or not
3. To create an [Angular](https://angular.io/) `library` run: `npx nx g @nrwl/angular:lib LIB_NAME`
	 > 1. `APP_NAME` - name of the application
4. To create an [Angular](https://angular.io/) `publishable library` run `npx nx g @nrwl/angular:lib LIB_NAME --publishable --importPath=NPM_NAME`
	 > 1. `APP_NAME` - name of the application
	 > 2. `--publishable` - to create build target in the `angular.json` or `workspace.json` which is the same
	 > 3. `--importPath=NPM_NAME` - name of npm package

