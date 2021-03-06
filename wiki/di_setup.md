# Dependency Injection with TDD
This guide gets you started on how to setup dependency injection and unit tests to your project.

## DI with [Inversify.js](http://inversify.io/) and Tests with [Moq.ts](https://github.com/dvabuzyarov/moq.ts)
Dinoloop already has a [dinoloop-inversify-starter](https://github.com/ParallelTask/dinoloop-inversify-starter) project for inversify and moq.ts setup. Here are few steps to setup:
```
git clone https://github.com/ParallelTask/dinoloop-inversify-starter.git
cd dinoloop-inversify-starter
npm install
npm start
```
Navigate to [http:localhost:8088/api/home/](http:localhost:8088/api/home/) in browser

## Project Structure and Files
Open project in your favorite editor (Recommended [VSCode](https://code.visualstudio.com/)). You will find following application structure and files in order to work with Dinoloop.

### src/app/container/inversify.decorate.ts
Inversify.js expects the parent/inherited classes to have `@injectable`. This file must be imported specific to inversify.js
```
import { decorate, injectable } from 'inversify';
import { 
    ApiController, 
    ErrorController, 
    RequestStartMiddleware, 
    RequestStartMiddlewareAsync, 
    RequestEndMiddleware,
    RequestEndMiddlewareAsync,
    Middleware,
    MiddlewareAsync,
    ActionFilter,
    ActionFilterAsync,
    ErrorMiddleware,
    ErrorMiddlewareAsync,
    ResultFilter,
    ResultFilterAsync,
    ExceptionFilter,
    ExceptionFilterAsync,
    IUserIdentity,
    UserIdentity
} from 'dinoloop';

/**
 * Builtin abstract classes of Dinoloop, must go through decorate().
 * These statements are specific to Inversify.js.
 * Inverisfy expects the inherited members to have @injectable()
 */
decorate(injectable(), ApiController);
decorate(injectable(), ErrorController);
decorate(injectable(), RequestStartMiddleware);
decorate(injectable(), RequestStartMiddlewareAsync);
decorate(injectable(), RequestEndMiddleware);
decorate(injectable(), RequestEndMiddlewareAsync);
decorate(injectable(), Middleware);
decorate(injectable(), MiddlewareAsync);
decorate(injectable(), ActionFilter);
decorate(injectable(), ActionFilterAsync);
decorate(injectable(), ErrorMiddleware);
decorate(injectable(), ErrorMiddlewareAsync);
decorate(injectable(), ResultFilter);
decorate(injectable(), ResultFilterAsync);
decorate(injectable(), ExceptionFilter);
decorate(injectable(), ExceptionFilterAsync);
decorate(injectable(), IUserIdentity);
decorate(injectable(), UserIdentity);
```
### src/app/container/app.container.ts
Here you create the DI container and bind the components that includes *controllers/services/middlewares ...*. You can read component bind on [inversify](https://github.com/inversify/InversifyJS/tree/master/wiki).
```
let container = new Container();
container.bind(IAboutService).to(AboutService);
container.bind(IAppConfigService).to(AppConfigService);
container.bind(HomeController).toSelf();
container.bind(ApplicationErrorController).toSelf();
container.bind(JsonResponse).toSelf();

export { container as AppContainer };
```
### src/app/controllers/home.controller.ts
This is a dinoloop controller responds to API requests.
### src/app/controllers/application.error.controller.ts
This is a dinoloop error-controller responds to unhandled exceptions/error thrown by application.
### src/app/middlewares
This is where you put your middleware classes.
### src/app/services
This is where you put your services.
### src/app/model
This is where you put your entities/models.
### test/helpers
This is where you put start-up code for your tests if required.
### test/unit
This is where you put your unit tests. You can find sample unit test case written using [moq.ts](https://github.com/dvabuzyarov/moq.ts) and [Jasmine](https://jasmine.github.io/).
```
import { Mock } from 'moq.ts';
import { IAppConfigService } from '../../../src/app/services/config/iapp.config.service';
import { AboutService } from '../../../src/app/services/about.service';
import { About } from '../../../src/app/model/about.model';

describe('services.about.service.spec', () => {
    it('get.returns_array_of_about_entity', () => {
        let mockAppConfig = new Mock<IAppConfigService>();
        let cut = new AboutService(mockAppConfig.object());
        let result = cut.get();

        expect(result).toEqual([new About()]);
    });
});
```
