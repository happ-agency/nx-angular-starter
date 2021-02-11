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

6. To instal [@angularclass/hmr](https://www.npmjs.com/package/@angularclass/hmr) (only for dev) run `pnpm i -D @angularclass/hmr`
		
	1. Create a file `src/environments/environment.hmr.ts` with: 

		 <details>
		 	<summary>environment.hmr.ts</summary>

			export const environment = {
				production: false,
				hmr: true
			};
		 </details>  
	
	2. Update `src/environments/environment.prod.ts` with:

		  <details>
		  	<summary>environment.prod.ts</summary>

		 	export const environment = {
		 		production: true,
		 		hmr: false
		 	};
		  </details>  
	
	3. Update `src/environments/environment.ts` with:

		 <details>
			<summary>environment.ts</summary>

			export const environment = {
				production: false,
				hmr: false
			};
		 </details>  
	
	4. Update `workspace.json` with:

		  <details>
		 		<summary>workspace.json</summary>

				"build": {
					"configurations": {
						...
						"hmr": {
							"fileReplacements": [
								{
									"replace": "apps/APP_NAME/src/environments/environment.ts",
									"with": "apps/APP_NAME/src/environments/environment.hmr.ts"
								}
							]
						}
					}
				},
				...
				"serve": {
					"configurations": {
						...
						"hmr": {
							"hmr": true,
							"browserTarget": "APP_NAME:build:hmr"
						}
					}
				}
		  </details>  
	
	5. Update `src/tsconfig.app.json` with

		 <details>
				<summary>tsconfig.app.json</summary>

			 {
				 ...
				 "compilerOptions": {
					 ...
					 "types": ["node"]
				 },
			 }
		 </details> 
		 
	6. Create a file `src/hmr.ts` with:
		 
		<details>
			<summary>hmr.ts</summary>
	
			import { NgModuleRef, ApplicationRef } from '@angular/core';
			import { createNewHosts } from '@angularclass/hmr';
			
			export const hmrBootstrap = (module: any, bootstrap: () => Promise<NgModuleRef<any>>) => {
				let ngModule: NgModuleRef<any>;
				module.hot.accept();
				bootstrap().then(mod => ngModule = mod);
				module.hot.dispose(() => {
					const appRef: ApplicationRef = ngModule.injector.get(ApplicationRef);
					const elements = appRef.components.map(c => c.location.nativeElement);
					const makeVisible = createNewHosts(elements);
					ngModule.destroy();
					makeVisible();
				});
			};
		</details>  
	
	7. Update `src/main.ts` with:

		<details>
			<summary>main.ts</summary>
			 
			import { enableProdMode } from '@angular/core';
			import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';
		 
			import { AppModule } from './app/app.module';
			import { environment } from './environments/environment';
			import { hmrBootstrap } from './hmr';
		 
			if (environment.production) {
				enableProdMode();
			}
			
			if (environment.hmr) {
				if (module[ 'hot' ]) {
		 			hmrBootstrap(module, () => platformBrowserDynamic().bootstrapModule(AppModule));
				} else {
					console.error('HMR is not enabled for webpack-dev-server!');
					console.log('Are you using the --hmr flag for ng serve?');
				}
			} else {
				 document.addEventListener('DOMContentLoaded', () => {
					 platformBrowserDynamic()
						 .bootstrapModule(AppModule)
						 .catch((err) => console.error(err));
					 });
			}
		</details>  
	
	8. Add `"hmr": "nx serve --configuration hmr"` to your `package.json` scripts

## Related Projects:
1. [nx-starter](https://github.com/happ-agency/nx-starter)
2. [nx-web-starter](https://github.com/happ-agency/nx-web-starter)
3. [nx-nestjs-starter](https://github.com/happ-agency/nx-nestjs-starter)
4. [nx-angular-nestjs-starter](https://github.com/happ-agency/nx-angular-nestjs-starter)
