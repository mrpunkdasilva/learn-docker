# Lab 2: Exercícios Práticos 🏋️‍♀️

```ascii
╔═══════════════════════════════════════════════════════════╗
║ EXERCISE STATUS: TREINANDO...                             ║
║ DIFICULDADE: ★★★☆☆                                      ║
║ TEMPO ESTIMADO: 90 minutos                               ║
║ WAIFU ASSISTANT: Exercise-chan                           ║
╚═══════════════════════════════════════════════════════════╝
```

## Preparação 🔧

### Requisitos
- Lab 2 Setup completo
- Docker Compose funcionando
- Ambiente de desenvolvimento configurado
- Editor de código pronto

### Verificação Inicial
```bash
# Verifique o ambiente
docker compose ps

# Teste os serviços
curl http://localhost:3000
curl http://localhost:5000/health
```

## Exercício 1: Frontend Development 🎨

### Objetivo
Desenvolver uma interface React com hot-reload e integração com backend.

### Passos
1. **Setup do Frontend**
```bash
# Entre no diretório frontend
cd frontend

# Crie novo projeto React
npm create vite@latest . -- --template react-ts

# Instale dependências
npm install axios @mui/material @emotion/react @emotion/styled
```

2. **Componente de Lista**
```typescript
// src/components/TaskList.tsx
import { useState, useEffect } from 'react';
import axios from 'axios';

interface Task {
  id: number;
  title: string;
  completed: boolean;
}

export const TaskList = () => {
  const [tasks, setTasks] = useState<Task[]>([]);

  useEffect(() => {
    axios.get('http://localhost:5000/tasks')
      .then(response => setTasks(response.data));
  }, []);

  return (
    <div>
      {tasks.map(task => (
        <div key={task.id}>
          <input
            type="checkbox"
            checked={task.completed}
            onChange={() => {/* TODO */}}
          />
          {task.title}
        </div>
      ))}
    </div>
  );
};
```

## Exercício 2: Backend API 🔧

### Objetivo
Criar uma API REST com Flask e integração com PostgreSQL.

### Passos
1. **Setup do Backend**
```bash
# Entre no diretório backend
cd backend

# Crie o arquivo requirements.txt
cat > requirements.txt << EOL
flask==2.0.1
flask-sqlalchemy==2.5.1
psycopg2-binary==2.9.1
flask-cors==3.0.10
python-dotenv==0.19.0
EOL

# Instale dependências
pip install -r requirements.txt
```

2. **API REST**
```python
# app.py
from flask import Flask, request, jsonify
from flask_sqlalchemy import SQLAlchemy
from flask_cors import CORS
import os

app = Flask(__name__)
CORS(app)

app.config['SQLALCHEMY_DATABASE_URI'] = os.getenv('DATABASE_URL')
db = SQLAlchemy(app)

class Task(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    title = db.Column(db.String(100), nullable=False)
    completed = db.Column(db.Boolean, default=False)

@app.route('/tasks', methods=['GET'])
def get_tasks():
    tasks = Task.query.all()
    return jsonify([{
        'id': task.id,
        'title': task.title,
        'completed': task.completed
    } for task in tasks])

@app.route('/tasks', methods=['POST'])
def create_task():
    data = request.get_json()
    task = Task(title=data['title'])
    db.session.add(task)
    db.session.commit()
    return jsonify({
        'id': task.id,
        'title': task.title,
        'completed': task.completed
    }), 201
```

## Exercício 3: Database Integration 💾

### Objetivo
Configurar PostgreSQL e migrations.

### Passos
1. **Schema Inicial**
```sql
-- database/init.sql
CREATE TABLE IF NOT EXISTS task (
    id SERIAL PRIMARY KEY,
    title VARCHAR(100) NOT NULL,
    completed BOOLEAN DEFAULT FALSE
);

-- Dados iniciais
INSERT INTO task (title) VALUES
    ('Aprender Docker'),
    ('Dominar Docker Compose'),
    ('Implementar CI/CD');
```

2. **Migrations com Alembic**
```bash
# Instale Alembic
pip install alembic

# Inicialize
alembic init migrations

# Crie primeira migration
alembic revision -m "create_task_table"
```

## Exercício 4: Integration & Testing 🔄

### Objetivo
Integrar todos os serviços e implementar testes.

### Passos
1. **Testes de Frontend**
```bash
# Instale dependências de teste
npm install --save-dev vitest @testing-library/react

# Crie teste
npm test
```

2. **Testes de Backend**
```bash
# Instale pytest
pip install pytest

# Execute testes
pytest tests/
```

## Desafios Extras 🌟

### 1. Cache Implementation
```yaml
# docker-compose.yml
services:
  redis:
    image: redis:alpine
    ports:
      - "6379:6379"
```

### 2. Logging Enhancement
```python
# backend/logging_config.py
import logging

logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s'
)
```

### 3. CI Pipeline
```yaml
# .github/workflows/ci.yml
name: CI

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Build and test
        run: docker compose -f docker-compose.test.yml up --exit-code-from tests
```

## Checklist de Conclusão ✅

### Frontend
- [ ] Componentes React implementados
- [ ] Estilização com Material-UI
- [ ] Integração com API
- [ ] Testes unitários

### Backend
- [ ] API REST funcional
- [ ] Conexão com banco de dados
- [ ] Migrations configuradas
- [ ] Testes de integração

### Database
- [ ] Schema inicial criado
- [ ] Dados de exemplo inseridos
- [ ] Backup configurado
- [ ] Migrations aplicadas

## Exercise-chan Tips 💡

> **Exercise-chan diz:** "Lembre-se de usar o comando `docker compose logs -f` para acompanhar os logs em tempo real durante o desenvolvimento! 📝"
{style="tip"}

> **Exercise-chan alerta:** "Sempre valide as respostas da API e trate os erros adequadamente no frontend! 🔒"
{style="warning"}

## Próximos Passos 🎯

1. [Docker Compose Deep Dive](docker-compose-deep-dive.md)
2. [Development Best Practices](development-best-practices.md)
3. [Lab 3: Production](lab-3-intro.md)

## Recursos Adicionais 📚

- [React Documentation](https://react.dev)
- [Flask Documentation](https://flask.palletsprojects.com)
- [PostgreSQL Documentation](https://www.postgresql.org/docs/)
- [Docker Compose Networking](https://docs.docker.com/compose/networking/)

> **Sensei's Note:** "A prática leva à perfeição, mas apenas se você estiver praticando da maneira correta! 🎯"
{style="note"}