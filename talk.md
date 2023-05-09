

 Informações Básicas

## O que é? E por que ligar?

A Standard Template Library do C++,
normalmente chamada de STL,
é um conjunto de funções e estruturas de dados versáteis e eficientes,
um toolkit ótimo para a resolução de problemas de diversos tipos na maratona.
Com a STL podemos evitar ter que reescrever os blocos básicos toda vez que vamos resolver um exercício qualquer
(um dos maiores diferenciais de C++ comparado a C).


## O que é esse tal de template?

O “T” em STL se refere a um recurso muito comum do C++ chamado template. Templates permitem que você crie funções e tipos de dados com “buracos” ou “placeholders” em seus sistemas de tipos. O uso de uma função com template normalmente tem a seguinte aparência:

```
// definição da função
template <class T1, class T2>
T1 my_function(T1 x, T1 y, T2 z) {
	cout << z;
	return x + y;
}

// uso
my_function<int, char>(10, 20, '+');

```

Apesar de definir templates em maratonas não ser extremamente comum, é muito relevante ser capaz de ler templates, já que essencialmente tudo na Standard **Template** Library os utiliza.

# `std::vector<T>`

O primeiro tipo que iremos comentar é o `std::vector`. Um vector é essencialmente um vetor dinâmico que pode ter seu tamanho modificado após criado. Para isso, é importante saber que o vector possui um `size`, o número de elementos inseridos, e uma `capacity`, o número de elementos que podem ser inseridos sem que o vector precise automaticamente alocar mais memória.

## Criação de um vector

Existem muitos jeitos de criar vectors, porém aqui seguem os mais comuns

```
// vector vazio
vector<int> numbers;

// vector com 10 strings vazias
// nota: as vezes as pessoas usam {}
//       ao invés de (), ambos funcionam,
//       pura preferência
vector<string> names(10);

// vector com 3 long longs inicializados com 7.
vector<long long> lucky{3, 7};

// vector vazio com capacidade 40
vector<string> incoming;
incoming.reserve(40);

```

## Inserção e Acesso

`vector`s podem ser acessados exatamente como vetores normais, usando o operatdor [], o tamanho pode ser verificado com  `.size()`:

```
vector<int> numbers(10);
cout << numbers.size(); // 10

for (int i = 0; i < 10; i++) {
	numbers[i] = 100 * i;
	cout << numbers[i];
}
// ponteiros também funcionam
int* p = &numbers[0];
cout << *(p + 1); // 100

```

Uma outra operação comum de vectors é o `push_back`, que insere um elemento no final do vector em $O(1)$ (nota: as vezes um push_back precisa aumentar a capacidade e percorrer o vetor, quando `size = capacity`, mas isto ocorre em raros casos).

```
vector<int> numbers;
numbers.reserve(0);

cout << numbers.size() // 0

for (int i = 0; i < 10; i++) {
	numbers.push_back(i * 100);
}

cout << numbers.size(); // 10

```

Existem muitas outras operações disponíveis neste tipo, então é recomendável dar uma olhadinha na documentação do cppreference para se informar melhore sobre.

# `std::map<K, V>`

Um map é uma estrutura de dados que permite a associação explícita de chaves a valores. Um jeito de pensar num map é como um vetor cujos índices não são necessariamente inteiros. Por exemplo, podemos ter mapas que associam strings a strings, strings a números, números a strings, etc. Uma outra característica de mapas é que o gasto de memória destes é proporcional à quantidade de chaves associadas neles, logo maps com chaves inteiras podem ser usados ao invés de vetores quando precisamos associar chaves em intervalos enormes e com relativamente poucas chaves.

Maps em C++ podem ser usados por meio da struct `std::map`. Os tipos que informamos para esta pelo template (dentro do `< >`) são em ordem, o tipo das chaves do mapa, e o tipo dos valores do mapa. Podemos inserir e buscar no mapa utilizando o operador `[]` assim como em vetores.

```
map<string, int> play_time;
play_time["soldier"] = 120;
play_time["engineer"] = 90;
play_time["medic"] = 70;

cout << play_time["engineer"]; // 90

map<int, string> rms = {
	{898362, "John Doe"},
	{177013, "Anne"},
	{18325, "Mark"}
};

cout << rms[18325]; // Mark

```

## `std::map` vs `std::unordered_map`

C++ possui dois tipos de mapas, os mapas ordenados (`std::map`) e os mapas não ordenados (`std::unordered_map`). A principal diferença entre os dois é a maneira em que são implementados. O `std::unordered_map` internamente utiliza vetores e transformações de chaves em inteiros para operar, isso significa que um mapa não ordenado tem suas operações de inserção e busca em O(1) na maior parte do tempo, porem isto signfica que as chaves estão guardadas numa ordem indefinida/aleatória. O `std::map` internamente sempre guarda as chaves de forma ordenada numa estrutura chamada de Árvore Binária. Por conta dessa ordenação interna, mapas ordenados podem possuem algumas garantias e operações adicionais, como `lower_bound` e `upper_bound`. [falar sobre min element max element].

# `std::set<T>`

- Inserção e busca eficientes
- Sem elementos repetidos
- `std::set` vs `std::unordered_set`

```
set<int> cool_numbers;
cool_numbers.insert(0);
cool_numbers.insert(42);
cool_numbers.insert(0);

cout << cool_numbers.find(42) != cool_numbers.end(); // true
cout << cool_numbers.size() // 2;

```

# `std::pair<A,B>`, `std::tuple<T1, T2, ...>`

- Tipo muito simples que consiste de dois elementos de tipo `T1` e `T2`
- Caso particular do `std::tuple`

```
pair<double, double> bhaskara(double a, double b, double c) {
	double delta = b * b - 4 * a * c;
	double sqrt_delta = sqrt(delta);

	return {(- b + sqrt_delta) / (2 * a),
	        (- b - sqrt_delta) / (2 * a)};
}

pair<double, double> result = bhaskara(1, -7, 12);

cout << result.first << '\\n'; // 3
cout << result.second << '\\n'; // 4

```

```
tuple<int, double, char> pies = make_tuple(3, 3.14, 'P');

cout << get<0>(pies) << '\\n';
cout << get<1>(pies) << '\\n';
cout << get<2>(pies) << '\\n';

```

# Práticas comuns usando STL

## `auto`

```
vector<int> get_some_vector();

long long x = 10; // long long
auto y = x; // long long
auto z = &y; // long long *

auto v = get_some_vector(); // vector<int>
// v = 10 <- erro

```

## Iteração

```
vector<int> v = get_some_vector();

for (int x : v) {
	cout << x << '\\n';
}

// ou

for (auto x : v) {
	cout << x << '\\n';
}

// safer

for (auto &x: v) {
	cout << x << '\\n';
}

```

## Iteração em mapas

```
map<int, string> rms = {
	{898362, "John Doe"},
	{177013, "Anne"},
	{18325, "Mark"}
};

for (auto &entry : rms) {
    cout << entry.first << ":" << entry.second;
}

for (auto &[key, value] : rms) {
   cout << key << ":" << value << endl;
}

```

## Iterators

- Valores que se comportam como ponteiros e permitem acesso aos elementos das estruturas de dados
- .`begin()` e .`end()`
- `std::begin()` e `std::end()`
- Aplica a `std::string`

```
std::vector<int> v = {10, 20, 30};

auto x = v.begin();

cout << *x << '\\n'; // 10
     << *(x + 1) << '\\n' // 20
     << *(x + 2) << '\\n' // 30
     << x + 3 == v.end() // true

```

- Possuem pelo menos *`` `++`, e `==`

```
set<int> s;
s.insert(3); s.insert(2); s.insert(1); s.insert(3);

auto x = s.begin();
cout << *x << '\\n'; // 1
x++;
cout << *x << '\\n' // 2

cout << x == s.find(2); // true
// x + 4 <- erro

```

```
set<int> s;

for (int i = 1; i <= 10 i++) {
    s.insert(i * 10);
}

auto x = s.lower_bound(35);
cout << *x << '\\n'; // 30

```

```
vector<int> v1{10};
int v2[10];

for (int i = 0; i < 10; i++) {
    v1.push_back(rand() % 100);
    v2[i] = rand() % 100;
}

sort(v1.begin(), v1.end());
// sort(v1.begin(), v1.begin() + 10);

sort(v2, v2 + 10);
// sort(begin(v2), end(v2));
// sort(&v2[0], &v2[10])

```

# There's More!

# `std::stack<T>`

- Last In First Out
- Ordem de retirada = Contrário da ordem de entrada
- Fazivel com `vector<T>.push_back` e `vector<T>.pop`

```
stack<string> tasks;

tasks.push("Play with MAC0329");
tasks.push("Study MAT105");
tasks.push("Watch Bixecamp");

cout << tasks.top() << '\\n'; // Watch Bixecamp
cout << tasks.size() << '\\n'; // 3

cout << tasks.top() << '\\n'; // Watch Bixecamp
tasks.pop();

cout << tasks.top() << '\\n'; // Study MAT105
tasks.pop();

cout << tasks.top() << '\\n'; // Play with MAC0329
tasks.pop();

```

# `std::queue<T>`

- First In, First Out
- Ordem de retirada = Ordem de entrada

```
queue<string> tasks;

tasks.push("Play with MAC0329");
tasks.push("Study MAT105");
tasks.push("Watch Bixecamp");

cout << tasks.top() << '\\n'; // Play With MAC0329
tasks.pop();

cout << tasks.top() << '\\n'; // Study MAT105
tasks.pop();

cout << tasks.top() << '\\n'; // Watch Bixecamp
tasks.pop();

```

# `std::priority_queue<T>`

- Ordem determinada exclusivamente pela prioridade
- Usar `std::pair` para prioridades específicas
- Inserir negativo para prioridades menor pra maior

```
priority_queue<int> values;

values.push(40);
values.push(80);
values.push(10);

cout << values.top() << '\\n'; // 80
values.pop();

cout << values.top() << '\\n'; // 40
values.pop();

cout << values.top() << '\\n'; // 10
values.pop();

```

```
priority_queue<pair<int, string>> values;

values.push({1, "Play with MAC0329"});
values.push({8001, "Watch BixeCamp"});
values.push({2, "Study MAT105"});

cout << values.top().second << '\\n'; // BixeCamp
values.pop();

cout << values.top().second << '\\n'; // MAT105
values.pop();

cout << values.top().second << '\\n'; // MAC0329
values.pop();

```

# Menções Honrrosas

`std::deque<T>`

- `push_back`, `pop_back` `back`, `push_front` `pop_front`, `front`

`std::multiset<T>`

- `insert`, `count`, `find`

# Header `<algorithm>`

A STL não se limita a containers e iterators, mas também provem uma grande lista de funções que implementam operações comuns em C++. A lista completa pode ser encontrada na documentação, e inclui multiplas funções de busca binária, ordenação, busca, contagem, cópia, preenchimento, remoção, rotação entre outros.

## Casos Notáveis

- `sort`, `reverse`

```
vector<string> terms = {"Use", "Words", "For", "Variable", "Names"};

sort(terms.begin(), terms.end());

for (auto &term : terms) { cout << term << '\\n'; } // For Names Use Variable Words

reverse(terms.begin(), terms.end());

for (auto &term : terms) { cout << term << '\\n'; } // Words Variable Use Names For

// sort(terms.rbegin(), terms.rend());

```

- `unique`, `erase`

```
vector<int> terms = {5, 2, 1, 5, 1, 3, 2};

sort(terms.begin(), terms.end()); // 1 1 2 2 3 5 5

auto new_end = unique(terms.begin(), terms.end()) ;

//               v terms.end()
// 1 2 3 5 ? ? ? ~
//         ^ new_end

terms.erase(new_end, terms.end());

for (auto &term : terms) { cout << term << '\\n'; } // 1 2 3 5

```

- `next_permutation`

```
string text = "past";

cout << text << '\\n';

for (int i = 0; i < 3; i++) {
    std::next_permutation(text.begin(), text.end());

    cout << text << '\\n';
}

// past, pats, psat, psta

```

# Mensagens de erro

Preparem-se.

```
int main() {

    map<string, set<string>> m;

    cout << m;

    return 0;
}

```

mais de 220 linhas de erro no GCC12.2.1
