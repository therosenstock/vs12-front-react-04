# TASK 04

## Introdução

Este pequeno artigo traz informações sobre conceitos abordados em React, como Imutabilidade de Estados, Forms simples e com React Hook Form e a validação Yup.

## Imutabilidade de Estados

A imutabilidade de estados é um conceito na programação onde, basicamente, os Objetos não podem ter seu estado alterado após sua criação, de forma que o Objeto é descartado quando o estado é alterado. Vale dizer também que quando um atributo imutável é instanciado, seu endereço de memória não pode ser alterado e nem podem ser reinicializados. As alterações no estado são feitas através de um novo objeto, preservando sempre o original.

No React, podemos observar esse conceito na construção dos componentes, que atualizam a interface sempre que o estado de um componente muda.

### Vantagens

- A imutabilidade de Estados melhora a segurança de dados e a eficiência do código, pois nele não é possível alterar por “acidente” um estado que está sendo compartilhado em vários threads;
- Torna possível a rastreabilidade, já que fica mais fácil monitorar como o estado evolui ao longo do tempo;
- Melhoria na performance, já que otimiza a renderização ao comparar os estados dos objetos e atualizar somente o que foi alterado.

## Aplicação em React:

```javascript
import React, { useState } from "react";

const MyComponent = () => {
  const [counter, setCounter] = useState(0);

  const incrementCounter = () => {
    setCounter(counter + 1);
  };

  return (
    <div>
      <h1>Counter: {counter}</h1>
      <button onClick={incrementCounter}>Increment</button>
    </div>
  );
};

export default MyComponent;
```

No exemplo, o componente MyComponent usa o Hook useState para manter o estado do contador, que é imutável e não pode ser alterado diretamente, somente através da função setCounter, que recebe um novo valor para o contador e atualiza o estado do componente.

## Formulário simples

Na construção de um formulário “cru”, você cria os componentes na mão ou com alguma biblioteca, como o Material UI e a manipulação desses dados é feita através dos handlers, que são funções que alteram o estado de nome e email e são chamadas a cada vez o usuário digita algo nos campos de entrada. No caso do submit, há o prevent default para evitar o recarregamento da página e um console.log que mostra uma mensagem com os dados submetidos.

Segue abaixo um exemplo:

```javascript
import React, { Component, ChangeEvent, FormEvent } from "react";

interface State {
  name: string;
  email: string;
}

class FormExample extends Component<{}, State> {
  constructor(props: {}) {
    super(props);
    this.state = {
      name: "",
      email: "",
    };
  }

  handleNameChange = (event: ChangeEvent<HTMLInputElement>) => {
    this.setState({ name: event.target.value });
  };

  handleEmailChange = (event: ChangeEvent<HTMLInputElement>) => {
    this.setState({ email: event.target.value });
  };

  handleSubmit = (event: FormEvent<HTMLFormElement>) => {
    event.preventDefault();
    console.log("Form submitted with data:", this.state);
  };

  render() {
    return (
      <div>
        <h2>Form Example</h2>
        <form onSubmit={this.handleSubmit}>
          <div>
            <label>Name:</label>
            <input
              type="text"
              value={this.state.name}
              onChange={this.handleNameChange}
            />
          </div>
          <div>
            <label>Email:</label>
            <input
              type="email"
              value={this.state.email}
              onChange={this.handleEmailChange}
            />
          </div>
          <button type="submit">Submit</button>
        </form>
      </div>
    );
  }
}

export default FormExample;
```

## React Hook Form

O Hook Form é uma biblioteca para gerenciar formulários que utiliza de Hooks como useState e useEffect para simplificar e otimizar o processo de criação, validação e gerenciamento de formulários, porém os componentes são gerenciados pela biblioteca. Como vantagens, temos uma melhor performance, um alto grau de customização, uma integração simples e uma simples validação com tratamento de erros.

Antes de mais nada, para usar o React Hook Form, é necessário executar este comando no terminal do seu projeto:

`npm install react-hook-form`

E importar nos seus documentos, aplicando da seguinte forma:

```javascript
import React from "react";
import { useForm } from "react-hook-form";

function FormExample() {
  const { register, handleSubmit, errors } = useForm();

  const onSubmit = (data) => {
    console.log(data);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <input type="text" name="name" ref={register({ required: true })} />
      {errors.name && <span>This field is required</span>}

      <input
        type="email"
        name="email"
        ref={register({ required: true, pattern: /^\S+@\S+$/i })}
      />
      {errors.email && <span>Invalid email</span>}

      <button type="submit">Submit</button>
    </form>
  );
}

export default FormExample;
```

O Hook Form inicia o formulário em “FormExample” e fornece os métodos:

- **Register** que é usado para conectar campos do formulário ao estado interno e passar algumas validações, no return é possível passar a informação “required: true” e um pattern para definir as entradas aceitas. São aceitas as seguintes validações: _required, min, max, minLength, maxLength, pattern, validate._
- **HandleSubmit** que é usado para submissão do formulário e receber os dados;
- **Errors** que contém os erros de validação associados ao formulário.

Também há o **Controller**, iniciado com _control_, que é utilizado para encapsular campos de entrada controlados:

```javascript
import React from "react";
import { useForm, Controller } from "react-hook-form";

function FormExample() {
  const { handleSubmit, control } = useForm();

  const onSubmit = (data) => {
    console.log(data);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <Controller
        name="name"
        control={control}
        defaultValue=""
        render={({ field }) => <input {...field} />}
      />

      <Controller
        name="email"
        control={control}
        defaultValue=""
        render={({ field }) => <input {...field} />}
      />

      <button type="submit">Submit</button>
    </form>
  );
}

export default FormExample;
```

Nele, podemos passar o nome do campo, o valor inicial do campo (defaultValue), o control que recebe o controle do formulário, o render que recebe uma função para colocar o campo em tela, passando o objeto field que contém as informações do campo.

## Validação YUP

Frequentemente empregado com React Hook Form, o YUP é uma biblioteca de validação de formulários, que fornece uma maneira simples e declarativa de definir as regras de validação para seus dados, garantindo que eles atendam as expectativas antes de serem enviados ou processados.

Antes de mais nada, para utilizar o Yup, devemos instalá-lo no projeto:

```
npm i yup
```

Com o Yup, podemos definir os **Schemas**,q que são esquemas para definir a estrutura dos dados de cada campo e as regras de validação associadas a ele, como o formato se é string, data, email, um número positivo, além de poder configurar mensagens e limitações ao input como torna-lo obrigatório.

```javascript
const addressSchema = yup.object().shape({
email: yup
      .string()
      .email(“Invalid email”)
      .required(“Email is required”),
  fullName: yup
      .string()
      .required(),
  houseNumber: yup
      .number()
      .required()
      .positive()
      .integer(),
  address: yup
      .string()
      .required(),
  postCode: yup
      .string()
      .required(),
  timestamp: yup
      .date()
      .default(() => (new Date()),
   }),});

```

Também podemos fazer uma validação se os dados seguem em conformidade com o schema, com o isValid, veja um exemplo completo abaixo:

```javascript
function FormExample() {
  const { handleSubmit, register, formState, setError } = useForm();

  const onSubmit = (data) => {
    addressSchema.isValid(data).then((isValid) => {
      if (isValid) {
        console.log("Form is valid:", data);
      } else {
        setError("form", {
          type: "manual",
          message: "Form is not valid according to schema",
        });
      }
    });
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <input type="text" name="street" placeholder="Street" ref={register} />
      <input type="text" name="city" placeholder="City" ref={register} />
      <input
        type="text"
        name="postalCode"
        placeholder="Postal Code"
        ref={register}
      />

      {formState.errors.form && <p>{formState.errors.form.message}</p>}

      <button type="submit">Submit</button>
    </form>
  );
}

export default FormExample;
```

Aqui, se o data recebe dados validos, de acordo com o schema especificado, ele entra na validação e mostra no console “Form is valid: true”, caso contrário, exibe um erro.
