# [Nx](https://nx.dev/) [Angular](https://angular.io/) Starter

> Based on the [happ/nx-starter](https://github.com/happ-agency/nx-starter)

## Content

1. [Angular](#angular)
2. [CLI](#cli)
3. [App](#app)
4. [Libray](#library)
5. [Pulbishable Library](#publishable-library)
6. [Universal](#universal)
7. [HMR](#hmr)
8. [i18n](#i18n)

## Angular
1. Install [Angular](https://angular.io/): `pnpm i -D @nrwl/angular`

## CLI
1. Install [Angular CLI]() (only for dev): `pnpm install -D @angular/cli`
2. Change `workspace.json` to `angular.json` with:
	1. `version: 2` to `version:1`
	2. `targets` to `architect`
	3. `generators` to `schematics`
	4. `executor` to `builder`

## App
1. Create an [Angular](https://angular.io/) `app`: `npx nx g @nrwl/angular:app APP_NAME --style=scss --routing=true`
	> 1. `APP_NAME` - name of the application
	> 2. `--style=scss` - default styles
	> 3. `--routing=true` - use routes by default or not
## Library
1. Create an [Angular](https://angular.io/) `library`: `npx nx g @nrwl/angular:lib LIB_NAME`
	> 1. `APP_NAME` - name of the application

## Publishable library
1. Create an [Angular](https://angular.io/) `publishable library`: `npx nx g @nrwl/angular:lib LIB_NAME --publishable --importPath=NPM_NAME`
	> 1. `APP_NAME` - name of the application
	> 2. `--publishable` - to create build target in the `angular.json` or `workspace.json` which is the same
	> 3. `--importPath=NPM_NAME` - name of npm package
	 
## Universal
1. Install [Angular Universal](https://angular.io/guide/universal): `pnpm i @nguniversal/express-engine`
2. Add [Angular Universal](https://angular.io/guide/universal) to your app: `npx nx g @nguniversal/express-engine:ng-add --client-project=APP_NAME`
	> 1. `APP_NAME` - name of the application
3. Replace `ng` to `nx` in `package.json` scripts: `dev:ssr`, `build:ssr`, `prerender`
4. Replace `ngExpressEngine` to `(ngExpressEngine as any)` in `server.ts` to avoid [this](https://github.com/angular/universal/issues/1210) error

## HMR
1. Install [@angularclass/hmr](https://www.npmjs.com/package/@angularclass/hmr) (only for dev): `pnpm i -D @angularclass/hmr`
2. Create a file `src/environments/environment.hmr.ts` with:
	<details>
		<summary>environment.hmr.ts</summary>
	
		export const environment = {
			production: false,
			hmr: true
		};
	</details>  
	 
3. Update `src/environments/environment.prod.ts` with:

	<details>
		<summary>environment.prod.ts</summary>
		
		export const environment = {
			production: true,
			hmr: false
		};
	</details>  

4. Update `src/environments/environment.ts` with:

	<details>
		<summary>environment.ts</summary>
		
		export const environment = {
			production: false,
			hmr: false
		};
	</details>  

5. Update `workspace.json` with:
	
	<details>
		<summary>workspace.json</summary>
	
		"build": {
			"configurations": {
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
		"serve": {
			"configurations": {
				"hmr": {
					"hmr": true,
					"browserTarget": "APP_NAME:build:hmr"
				}
			}
		}
	</details>  

6. Update `src/tsconfig.app.json` with

	<details>
		<summary>tsconfig.app.json</summary>
		
		"compilerOptions": {
	 		"types": ["node"]
		},
	</details> 

7. Create a file `src/hmr.ts` with:

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

8. Update `src/main.ts` with:

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

9. Add `"hmr": "nx serve --configuration hmr"` to your `package.json` scripts

## i18n

1. Install [@angular/localize](https://angular.io/guide/i18n): `pnpm i @angular/localize`
2. Add [@angular/localize](https://angular.io/guide/i18n) to your app: `npx nx g @angular/localize:ng-add`
3. Update your `angular.json` with:

	<details>
		<summary>angular.json</summary>
		
		...
		"i18n": {
			"sourceLocale": "en-US",
			"locales": {
				"ru": "apps/APP_NAME/src/i18n/messages.xlf"
			}
		},
		"targets": {
			"build": {
				"options": {
					"localize": true,
				}
					configurations: {
					"ru": {
						"localize": ["ru"]
					}
				}
			}
			"serve": {
				configurations: {
					"ru": {
						"localize": ["ru"]
					}
				}
			}
		}
	</details> 

4. Update your `package.json` with:

	<details>
		<summary>package.json</summary>
		
		"hmr": "nx serve --configuration hmr",
		"extract": "nx extract-i18n --output-path apps/APP_NAME/src/i18n",
		"dev:ru": "nx serve --configuration=ru"
	</details> 

5. Install [@ngx-i18nsupport/tooling](https://www.npmjs.com/package/@ngx-i18nsupport/tooling) run: `pnpm i @ngx-i18nsupport/tooling`
6. Add [@ngx-i18nsupport/tooling](https://www.npmjs.com/package/@ngx-i18nsupport/tooling) for your app run: `npx nx g @ngx-i18nsupport/tooling:ng-add  --i18nLocale=en --languages en,ru --i18nFormat=xlf`
	> 1. `--i18nLocale=en` - default locale
	> 2. `--languages=en,ru` - available languages (You can add more later)
	> 3. `--i18nFormat=xlf` - format of translate files
	 
## Related Projects:
1. [nx-starter](https://github.com/happ-agency/nx-starter)
2. [nx-web-starter](https://github.com/happ-agency/nx-web-starter)
3. [nx-nestjs-starter](https://github.com/happ-agency/nx-nestjs-starter)
4. [nx-angular-nestjs-starter](https://github.com/happ-agency/nx-angular-nestjs-starter)

## Additional Info

> You can check additional info in [nx-starter](https://github.com/happ-agency/nx-starter)
