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
5. To install [Angular Universal](https://angular.io/guide/universal) run: `pnpm i @nguniversal/express-engine`
	1. To add [Angular Universal](https://angular.io/guide/universal) for your app run: `npx nx g @nguniversal/express-engine:ng-add --client-project=APP_NAME`
		> 1. `APP_NAME` - name of the application
	2. Replace `ng` to `nx` in `package.json` scripts: `dev:ssr`, `build:ssr`, `prerender`
	3. Replace `ngExpressEngine` to `(ngExpressEngine as any)` in `server.ts` to avoid [this](https://github.com/angular/universal/issues/1210) error

## Related Projects:
1. [nx-starter](https://github.com/happ-agency/nx-starter)
2. [nx-web-starter](https://github.com/happ-agency/nx-web-starter)
3. [nx-nestjs-starter](https://github.com/happ-agency/nx-nestjs-starter)
4. [nx-angular-nestjs-starter](https://github.com/happ-agency/nx-angular-nestjs-starter)
