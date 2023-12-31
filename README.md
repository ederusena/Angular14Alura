## Formulario Reactive Data Driven

```ts
/// app.modules.ts
import { FormsModule, ReactiveFormsModule } from '@angular/forms';
imports: [
    ReactiveFormsModule
  ],

/// criar-pensamento.ts

  formulario!: FormGroup;

  constructor(
    private service: PensamentoService,
    private router: Router,
    private formBuilder: FormBuilder
  ) { }

  ngOnInit(): void {
    this.formulario = this.formBuilder.group({
      conteudo: ['Formulario reative'],
      autoria: [''],
      modelo: ['modelo1']
    })
  }
```

### Vinculacao do formulario reative

```text
Na tag <form> do html, é necessário fazer um property binding por meio da diretiva formGroup e atribuir a ela o valor da propriedade formulario, criada na classe typescript. Além disso, é necessário incluir em cada input a propriedade formControlName, com o nome declarado na classe.
```

```html
<form [formGroup]="formulario">

<input
  formControlName="conteudo"
>

formControlName="autoria"
formControlName="modelo"

<p class="conteudo">{{ formulario.get('conteudo')!.value }}</p>
<p class="autoria"><b>{{ formulario.get('autoria')!.value }}</b></p>
```

```text
O FormGroup representa um grupo de dados no formulário. Você pode ter vários grupos em um único formulário, o que facilita e organiza o controle dos dados.

O FormControlName é uma diretiva que sincroniza os controles em um formGroup através da sincronização por meio do nome da propriedade do formGroup, como fiz com o conteúdo, autoria e modelo.
```

### Validators

```ts
ngOnInit(): void {
    this.formulario = this.formBuilder.group({
      conteudo: ['', Validators.required],
      autoria: ['', Validators.required],
      modelo: ['modelo1', Validators.required],
    });
  }
```

## Validators pattern e minLength

```ts
ngOnInit(): void {
    this.formulario = this.formBuilder.group({
      conteudo: ['',
        Validators.compose(
          [
            Validators.required,
            Validators.pattern(/(.|\s)*\S(.|\s)*/),
          ]
        )
      ],
      autoria: ['',
        Validators.compose(
          [
            Validators.required,
            Validators.minLength(10)
          ]
        )
      ],
      modelo: ['modelo1',
        Validators.compose(
          [
            Validators.required
          ]
        )
      ],
    });
  }
```

## Exibindo mensagens de erro

```html
<div
  class="mensagem__erro"
  *ngIf="formulario.get('conteudo')?.errors?.['required'] && formulario.get('conteudo')?.touched"
>
  <span>O campo pensamento é obrigatório.</span>
</div>
<div
  class="mensagem__erro"
  *ngIf="formulario.get('conteudo')?.errors?.['minlength'] && formulario.get('conteudo')?.touched"
>
  <span>O pensamento deve ter entre 3 e 280 caracteres.</span>
</div>
```

## HttpParams Paginacao

```ts
listar(pagina: number): Observable<Pensamento[]> {
    const itensPorPagina = 6;
    let params = new HttpParams()
      .set('_page', pagina.toString())
      .set('_limit', itensPorPagina.toString());

    // return this.http.get<Pensamento[]>(
    //   `${this.API}?_page=${pagina}&_limit=${itensPorPagina}`
    // );

    return this.http.get<Pensamento[]>(this.API, { params });
  }
```

```ts
listaPensamentos: Pensamento[] = [];
  paginaAtual: number = 1;
  constructor(private service: PensamentoService) { }

  ngOnInit(): void {
    this.service.listar(this.paginaAtual).subscribe((listaPensamentos) => {
      this.listaPensamentos = listaPensamentos
    })
  }
```
