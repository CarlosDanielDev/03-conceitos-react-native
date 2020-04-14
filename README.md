# Dia 07 - Desafio Conceitos React Native

Data: Apr 14, 2020
Hora de início: 16:00
Hora de término: 17:00
Status: Finalizado

Hoje estou inciando o 3º desafio proposto pela Rocketseat na noja jornada GoStack!

Aqui estão as regras do Desafio:

- [x] `**Listar os repositórios da sua API**`: Deve ser capaz de criar uma lista de todos os repositórios que estão cadastrados na sua API com os campos `title`, `techs` e número de curtidas seguindo o padrão `**${repository.likes}**` curtidas, apenas alterando o número para ser dinâmico.
- [x] `**Curtir um repositório listado da API**`: Deve ser capaz de curtir um item na sua API através de um botão com o texto Curtir e deve atualizar o número de `**likes**` na listagem no `**mobile**`.

Aqui está a resolução do Primeiro tópico do desafio, fiz uma simples chamada a `**api**`, e listei seus itens utilizando o componente `**FlatList`:\*\*

```jsx
import React, { useState, useEffect } from "react";

import {
  SafeAreaView,
  View,
  FlatList,
  Text,
  StatusBar,
  StyleSheet,
  TouchableOpacity,
} from "react-native";

import api from "./services/api"; // importando a API

export default function App() {
  const [repositories, setRepositories] = useState([]); // criando estado

  useEffect(() => {
    // chamando API
    api.get("/repositories").then((response) => setRepositories(response.data));
  }, []);

  async function handleLikeRepository(id) {
    // function add clicks
  }

  return (
    <>
      <StatusBar barStyle="light-content" backgroundColor="#2273BF" />
      <SafeAreaView style={styles.container}>
        {/* Utilizando a FlatList pra listar os Repositories da API*/}
        <FlatList
          data={repositories}
          keyExtractor={(repo) => repo.id}
          renderItem={({ item: repo }) => (
            <>
              <View style={styles.repositoryContainer}>
                <Text style={styles.repository}>{repo.title}</Text>

                <View style={styles.techsContainer}>
                  {repo.techs.map((tech) => (
                    <Text key={tech} style={styles.tech}>
                      {tech}
                    </Text>
                  ))}
                </View>

                <View style={styles.likesContainer}>
                  <Text
                    style={styles.likeText}
                    // Remember to replace "1" below with repository ID: {`repository-likes-${repository.id}`}
                    testID={`repository-likes-${repo.id}`}
                  >
                    {repo.likes} curtidas
                  </Text>
                </View>

                <TouchableOpacity
                  style={styles.button}
                  onPress={() => handleLikeRepository(repo.id)}
                  // Remember to replace "1" below with repository ID: {`like-button-${repository.id}`}
                  testID={`like-button-${repo.id}`}
                >
                  <Text style={styles.buttonText}>Curtir</Text>
                </TouchableOpacity>
              </View>
            </>
          )}
        />
      </SafeAreaView>
    </>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: "#2273BF",
  },
  repositoryContainer: {
    marginBottom: 15,
    marginHorizontal: 15,
    backgroundColor: "#fff",
    padding: 20,
    borderRadius: 4,
  },
  repository: {
    fontSize: 32,
    fontWeight: "bold",
  },
  techsContainer: {
    flexDirection: "row",
    marginTop: 10,
  },
  tech: {
    fontSize: 12,
    fontWeight: "bold",
    marginRight: 10,
    backgroundColor: "#FF8000",
    paddingHorizontal: 10,
    paddingVertical: 5,
    color: "#fff",
    borderRadius: 4,
  },
  likesContainer: {
    marginTop: 15,
    flexDirection: "row",
    justifyContent: "space-between",
  },
  likeText: {
    fontSize: 14,
    fontWeight: "bold",
    marginRight: 10,
  },
  button: {
    marginTop: 10,
  },
  buttonText: {
    fontSize: 14,
    fontWeight: "bold",
    marginRight: 10,
    color: "#fff",
    backgroundColor: "#2273BF",
    padding: 15,
    borderRadius: 4,
  },
});
```

Observe o Resultado abaixo:
<img src="https://i.imgur.com/kYsE5GB.jpg" width="200" height="400"/>
A resolução do Segundo tópico do desafio foi a seguinte:

```jsx
async function handleLikeRepository(id) {
  const response = await api.post(`/repositories/${id}/like`);
  const repoIndex = repositories.findIndex((repo) => repo.id === id);
  const newArray = repositories;
  newArray[repoIndex] = response.data;

  setRepositories([...newArray]);
}
```

Pegando `ID` do repositório como parâmetro e adicionando a entidade `**like**` para cada `**repository**`.

Chamando a rota de adição de novos `likes`, para que não haja necessidade de `**refresh**` na aplicação, pegando `id` do repositório selecionado encontro o `**index**` dele no `**array**` de `**repositories**`, e com a resposta da `**api**` coloco o item modificado na sua devida posição.

Observe o resultado abaixo:

<img src="https://i.imgur.com/BIEZCJM.gif" width="200" height="400"/>
