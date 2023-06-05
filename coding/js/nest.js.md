[repl](https://docs.nestjs.com/recipes/repl)
```ts
// repl.ts
import { repl } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap() {
  await repl(AppModule); // or any module of interest
}
bootstrap();
// then run npm run start -- --entryFile repl
```

swagger json from nestjs
```bash
http://path/to/swagger-json # add json at the end
```