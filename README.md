# web-service
import jakarta.persistence.*;

@Entity
public class Aluno {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String nome;
    private String email;

    // Getters e Setters
}
import jakarta.persistence.*;

@Entity
public class Disciplina {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String nome;

    // Getters e Setters
}
import jakarta.persistence.*;
import java.time.LocalDate;
import java.util.List;

@Entity
public class Formulario {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private LocalDate dataCriacao;

    @ManyToOne
    private Disciplina disciplina;

    @OneToMany(mappedBy = "formulario", cascade = CascadeType.ALL)
    private List<Pergunta> perguntas;

    // Getters e Setters
}
import jakarta.persistence.*;
import java.time.LocalDate;
import java.util.List;

@Entity
public class Formulario {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private LocalDate dataCriacao;

    @ManyToOne
    private Disciplina disciplina;

    @OneToMany(mappedBy = "formulario", cascade = CascadeType.ALL)
    private List<Pergunta> perguntas;

    // Getters e Setters
}
import jakarta.persistence.*;
import java.time.LocalDate;
import java.util.List;

@Entity
public class Formulario {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private LocalDate dataCriacao;

    @ManyToOne
    private Disciplina disciplina;

    @OneToMany(mappedBy = "formulario", cascade = CascadeType.ALL)
    private List<Pergunta> perguntas;

    // Getters e Setters
}
import jakarta.persistence.*;

@Entity
public class Resposta {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @ManyToOne
    private Pergunta pergunta;

    @ManyToOne
    private Aluno aluno;

    private String textoResposta;
    private Double nota;

    // Getters e Setters
}
import jakarta.persistence.*;
import java.time.LocalDate;

@Entity
public class Frequencia {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @ManyToOne
    private Aluno aluno;

    private LocalDate dataPresenca;
    private Boolean presente;

    // Getters e Setters
}
import org.springframework.data.jpa.repository.JpaRepository;

public interface AlunoRepository extends JpaRepository<Aluno, Long> {}
public interface DisciplinaRepository extends JpaRepository<Disciplina, Long> {}
public interface FormularioRepository extends JpaRepository<Formulario, Long> {}
public interface PerguntaRepository extends JpaRepository<Pergunta, Long> {}
public interface RespostaRepository extends JpaRepository<Resposta, Long> {}
public interface FrequenciaRepository extends JpaRepository<Frequencia, Long> {}
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/alunos")
public class AlunoController {

    @Autowired
    private AlunoRepository alunoRepository;

    @GetMapping
    public List<Aluno> listarTodos() {
        return alunoRepository.findAll();
    }

    @PostMapping
    public Aluno criarAluno(@RequestBody Aluno aluno) {
        return alunoRepository.save(aluno);
    }

    @GetMapping("/{id}")
    public ResponseEntity<Aluno> buscarPorId(@PathVariable Long id) {
        return alunoRepository.findById(id)
            .map(ResponseEntity::ok)
            .orElse(ResponseEntity.notFound().build());
    }
}
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/formulario/{id}/respostas")
public class RespostaController {

    @Autowired
    private RespostaRepository respostaRepository;

    @Autowired
    private FormularioRepository formularioRepository;

    @PostMapping
    public ResponseEntity<Resposta> enviarResposta(@PathVariable Long id, @RequestBody Resposta resposta) {
        return formularioRepository.findById(id).map(formulario -> {
            respostaRepository.save(resposta);
            return ResponseEntity.ok(resposta);
        }).orElse(ResponseEntity.notFound().build());
    }
}
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=
spring.h2.console.enabled=true
spring.jpa.hibernate.ddl-auto=update
