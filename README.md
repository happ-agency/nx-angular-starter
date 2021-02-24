# [Nx](https://nx.dev/) [Angular](https://angular.io/) Starter

> Based on the [happ/nx-starter](https://github.com/happ-agency/nx-starter)

## Content

1. [Nrwl angular](#nrwl-angular)
2. [Nrwl angular app](#nrwl-angular-app)
3. [Nrwl angular library](#nrwl-angular-library)
4. [Nrwl angular publishable library](#nrwl-angular-publishable-library)
5. [Angular localize](#angular-localize)
6. [Ngx i18n support tooling](#ngx-i18n-support-tooling)
7. [Xliffmerge autotranslate feature](#xliffmerge-autotranslate-feature)
8. [Angular universal](#angular-universal)
9. [Angular hmr](#angular-hmr)
10. [Css reset](#css-reset)
11. [Normalize](#normalize)
12. [Angular material](#angular-material)
13. [Tailwind](#tailwind)
14. [Used libraries](#used-libraries)
15. [Related projects](#related-projects)

## Nrwl Angular
> [Nrwl angular](https://www.npmjs.com/package/@nrwl/angular)
1. Install [@nrwl/angular](https://www.npmjs.com/package/@nrwl/angular) only for dev: `pnpm i -D @nrwl/angular`

## Nrwl angular app
> [Nrwl angular application](https://nx.dev/latest/angular/angular/application)

1. Create an [Nrwl angular application](https://nx.dev/latest/angular/angular/application): `pnpx nx g @nrwl/angular:app APP_NAME --style=scss --routing=true`
	> 1. `APP_NAME` - name of the application
	> 2. `--style=scss` - default styles
	> 3. `--routing=true` - use routes by default or not
2. Create:
	1. `environment.example.ts` from `environment.ts`
	2. `environment.prod.example.ts` from `environment.prod.ts`
3. Update `.gitignore` with:
	```
	# APP_NAME Angular Environment
	/apps/APP_NAME/src/environments/environment.ts
	/apps/APP_NAME/src/environments/environment.prod.ts
 	```
 
	> Use `environment.*.example.ts` to push on git.

## Nrwl angular library
> [Nrwl angular library](https://nx.dev/latest/angular/angular/library)

1. Create an [Nrwl angular library](https://nx.dev/latest/angular/angular/library): `pnpx nx g @nrwl/angular:lib LIB_NAME`
	> 1. `APP_NAME` - name of the application

## Nrwl angular publishable library
> [Nrwl angular library](https://nx.dev/latest/angular/angular/library)

1. Create an [Nrwl angular publishable library](https://nx.dev/latest/angular/angular/library): `pnpx nx g @nrwl/angular:lib LIB_NAME --publishable --importPath=NPM_NAME`
	> 1. `APP_NAME` - name of the application
	> 2. `--publishable` - to create build target in the `workspace.json` which is the same
	> 3. `--importPath=NPM_NAME` - name of npm package

## Angular localize
> [Angular localize](https://angular.io/guide/i18n)

1. Install [@angular/localize](https://angular.io/guide/i18n) only for dev: `pnpm i -D @angular/localize`
2. Update `apps/app-name/src/polyfills.ts` with:
	 <details>
		 <summary>polyfills.ts</summary>

		import '@angular/localize/init';
	 	...
	 </details>
3. Update `workspace.json` with:

	 <details>
	 	<summary>workspace.json</summary>
		
		{
	 		...
			"projects" {
	 			...
	 			"APP_NAME": {
					...
					"i18n": {
						"sourceLocale": "LANGUAGE"
					},
					"targets": {
						"build": {
							...
							configurations: {
								...
								"LANGUAGE": {
									"aot": true,
									"outputPath": "dist/apps/APP_NAME_LANGUAGE",
									"i18nFile": "apps/APP_NAME/src/i18n/messages.LANGUAGE.xlf",
									"i18nFormat": "xlf",
									"i18nLocale": "LANGUAGE"
								}
							}
						}
						"serve": {
							configurations: {
								"LANGUAGE": {
									"browserTarget": "app-name:build:LANGUAGE"
								}
							}
						}
					}
	 			}
	 		}
	 	}
	 </details> 

4. Update your `package.json` with:

	 <details>
	 	<summary>package.json</summary>
	 
	 	{
	 		...
	 		"scripts": {
	 			...
				"extract-i18n": "nx extract-i18n --output-path apps/APP_NAME/src/i18n",
				"dev:ru": "nx serve --configuration=ru"
	 		}
	 	}

	 </details> 

## Ngx-i18n support tooling
> [Ngx-i18n support tooling](https://www.npmjs.com/package/@ngx-i18nsupport/tooling)

1. Install [@ngx-i18nsupport/tooling](https://www.npmjs.com/package/@ngx-i18nsupport/tooling) only for dev: `pnpm i -D @ngx-i18nsupport/tooling`
2. Update your `workspace.json` with:

	 <details>
		<summary>workspace.json</summary>

		{
	 		...
			"projects" {
	 			...
				"APP_NAME" {
	 				...
					"targets" {
						...
						"xliffmerge": {
							"executor": "@ngx-i18nsupport/tooling:xliffmerge",
							"options": {
								"xliffmergeOptions": {
									"i18nFormat": "xlf",
									"srcDir": "apps/APP_NAME/src/i18n",
									"genDir": "apps/APP_NAME/src/i18n",
									"defaultLanguage": "en",
									"languages": [
										"en",
										"ru"
									]
								}
							}
	 					}
					}
				}
			}
	 	}
	 </details> 
	 
3. Update your `package.json` with:

	 <details>
	 	<summary>package.json</summary>

	 	{
	  		...
	 		"scripts": {
	 			...
	 			"xliffmerge": "nx run app-name:xliffmerge"
	 		}
	 	}
	 </details> 
	 
## Xliffmerge autotranslate feature
> [xliffmerge-autotranslate-feature](https://github.com/martinroob/ngx-i18nsupport/wiki/xliffmerge-autotranslate-feature)

1. Update `workspace.json` with:

	<details>
		<summary>workspace.json</summary>
	 
		{
			...
			"projects" {
				...
				"APP_NAME" {
					...
					"targets" {
						...
						"xliffmerge": {
						 	...
							"autotranslate": true,
							"apikey": "GOOGLE_API_KEY"
						}
					}
				}
			}
		}
	</details> 


## Angular Universal
> [Angular Universal](https://angular.io/guide/universal)

1. Install [Angular Universal](https://angular.io/guide/universal): `pnpm i @nguniversal/express-engine`
2. Add [Angular Universal](https://angular.io/guide/universal) to your app: `pnpx nx g @nguniversal/express-engine:ng-add --client-project=APP_NAME`
	> 1. `APP_NAME` - name of the application
3. Replace `ng` to `nx` in `package.json`
4. Replace `dist/app-name` to `dist/apps/app-name` in `package.json`, `server.ts` and `workspace.json`

## Angular HMR
> [Angular HMR](https://habr.com/ru/company/skillfactory/blog/527516/)

1. Update `src/tsconfig.app.json` with

	<details>
		<summary>tsconfig.app.json</summary>
		
		"compilerOptions": {
	 		"types": ["node"]
		},
	</details> 

2. Update `src/main.ts` with:

	<details>
		<summary>main.ts</summary>

		import { enableProdMode } from '@angular/core';
		import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';
		
		import { AppModule } from './app/app.module';
		import { environment } from './environments/environment';

		if (environment.production) {
			enableProdMode();
		}
	 
		function bootstrap() {
			platformBrowserDynamic()
				.bootstrapModule(AppModule)
				.catch((err) => console.error(err));
		}
		
		if (module['hot']) {
			bootstrap();
		} else {
			document.addEventListener('DOMContentLoaded', () => {
				bootstrap();
			});
		}
	</details>  

3. Update `package.json` with:

	<details>
		<summary>package.json</summary>

		{
			...
			"scripts": {
				...
				 "dev:hmr": "nx serve --hmr"
			}
			...
		}
 	</details>
	
## Css reset
> [Css reset](https://meyerweb.com/eric/tools/css/reset/)

1. Create `apps/APP_NAME/src/assets/styles/reset.css` with:
	 <details>
	 <summary>reset.css</summary>

		html, body, div, span, applet, object, iframe,
		h1, h2, h3, h4, h5, h6, p, blockquote, pre,
		a, abbr, acronym, address, big, cite, code,
		del, dfn, em, img, ins, kbd, q, s, samp,
		small, strike, strong, sub, sup, tt, var,
		b, u, i, center,
		dl, dt, dd, ol, ul, li,
		fieldset, form, label, legend,
		table, caption, tbody, tfoot, thead, tr, th, td,
		article, aside, canvas, details, embed,
		figure, figcaption, footer, header, hgroup,
		menu, nav, output, ruby, section, summary,
		time, mark, audio, video {
			margin: 0;
			padding: 0;
			border: 0;
			font-size: 100%;
			font: inherit;
			vertical-align: baseline;
		}
		article, aside, details, figcaption, figure,
		footer, header, hgroup, menu, nav, section {
			display: block;
		}
		body {
			line-height: 1;
		}
		ol, ul {
			list-style: none;
		}
		blockquote, q {
			quotes: none;
		}
		blockquote:before, blockquote:after,
		q:before, q:after {
			content: '';
			content: none;
		}
		table {
			border-collapse: collapse;
			border-spacing: 0;
		}
	  </details>
2. Update `styles.scss` with: 

	 <details>
	 	<summary>styles.scss</summary>
	 
		...
	 	@import '~assets/styles/reset.css';
	 </details>

## Normalize
> [Normalize](https://necolas.github.io/normalize.css/)

1. Install [normalize](https://necolas.github.io/normalize.css/) `pnpm i normalize.css`
2. Update `styles.scss` with:

	<details>
		<summary>styles.scss</summary>

		...
	 	@import '~normalize.css';
	</details>

## Angular material
> [Angular material](https://www.npmjs.com/package/@angular/material)

1. Install [@angular/material](https://www.npmjs.com/package/@angular/material): `pnpm i @angular/material`
2. Add [@angular/material](https://www.npmjs.com/package/@angular/material) to app: `pnpx nx g @angular/material:ng-add`

## Tailwind
> [Tailwind](https://www.npmjs.com/package/@ngneat/tailwind)

1. Install [@ngneat/tailwind](https://www.npmjs.com/package/@ngneat/tailwind): `pnpm i @ngneat/tailwind`
2. Add [@ngneat/tailwind](https://www.npmjs.com/package/@ngneat/tailwind) to app: `pnpx nx g @ngneat/tailwind:ng-add`
3. Install [official plugins](https://tailwindcss.com/docs/plugins): `pnpm i @tailwindcss/typography @tailwindcss/forms @tailwindcss/line-clamp @tailwindcss/aspect-ratio`
2. Update `tailwind.config.js` with:

	 <details>
	 	<summary>tailwind.config.js</summary>

		module.exports = {
			...
			plugins: [
				require('@tailwindcss/typography'),
				require('@tailwindcss/forms'),
				require('@tailwindcss/line-clamp'),
				require('@tailwindcss/aspect-ratio'),
			],
		}
	 </details>

> ! IMPORTANT
> 
> Setup [Tailwind](#tailwind) after the [Angular Material](#angular-material). 
> 
> `Angular Material` schematics will error out if it detects a custom webpack

## Structure

 ```
	┌─ apps/ 
	│  ┌─ APP_NAME/
	│  │  ┌─ src/
	│  │  │  ┌─ app/
	│  │  │  │  ┌─ core/
	│  │  │  │  │  ┌─ layout/
	│  │  │  │  │  │  ┌─ core.coponent.html
	│  │  │  │  │  │  ├─ core.component.scss
	│  │  │  │  │  │  ├─ core.component.spec.ts
	│  │  │  │  │  │  └─ core.component.ts
	│  │  │  │  │  ├─ core.routing.module.ts
	│  │  │  │  │  └─ core.module.ts
	│  │  │  │  ├─ shared/
	│  │  │  │  │  └─ shared.module.ts
	│  │  │  │  ├─ */
	│  │  │  │  │  ┌─ layout/
	│  │  │  │  │  │  ┌─ *.coponent.html
	│  │  │  │  │  │  ├─ *.component.scss
	│  │  │  │  │  │  ├─ *.component.spec.ts
	│  │  │  │  │  │  └─ *.component.ts
	│  │  │  │  │  ├─ *.routing.module.ts
	│  │  │  │  │  └─ *.module.ts
	│  │  │  │  └── app.module.ts
	│  │  │  ├── assets/
	│  │  │  │  ┌─ images/
	│  │  │  │  └─ styles/
	│  │  │  ├─ environment/
	│  │  │  │  ┌─ environment.*.ts
	│  │  │  │  └─ environment.*.example.ts
	│  │  │  ├── i18n/
	├──┴──┴──┘  └─ messages.*.xlf
	├─ libs/ 
	│  ┌─ LIB_NAME/
	│  │  ┌─ src/
	│  │  │  ┌─ lib/
	│  │  │  ├── LIB_NAME.module.ts
	└──┴──┴──┘
 ```

## Used Libraries
1. [@nrwl/angular](https://www.npmjs.com/package/@nrwl/angular)
2. [@angular/cli](https://www.npmjs.com/package/@angular/cli)
3. [@angular/localize](https://angular.io/guide/i18n)
4. [@ngx-i18nsupport/tooling](https://www.npmjs.com/package/@ngx-i18nsupport/tooling)
5. [@nguniversal/express-engine](https://www.npmjs.com/package/@nguniversal/express-engine)
6. [@angularclass/hmr](https://www.npmjs.com/package/@angularclass/hmr)
7. [@angular/material](https://www.npmjs.com/package/@angular/material)
8. [@ngneat/tailwind](https://www.npmjs.com/package/@ngneat/tailwind)

## Nice to use libraries
1. [until-destroy](https://github.com/ngneat/until-destroy)

## Related Projects:
1. [nx-starter](https://github.com/happ-agency/nx-starter)
2. [nx-web-starter](https://github.com/happ-agency/nx-web-starter)
3. [nx-nestjs-starter](https://github.com/happ-agency/nx-nestjs-starter)
4. [nx-angular-nestjs-starter](https://github.com/happ-agency/nx-angular-nestjs-starter)

## Additional Info

> You can check additional info in [nx-starter](https://github.com/happ-agency/nx-starter)
