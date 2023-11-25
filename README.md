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
