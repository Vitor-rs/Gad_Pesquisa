Vou criar o módulo completo de extração e tratamento de PDFs para o Sistema GAD. Este módulo será fundamental para garantir que o texto chegue limpo e estruturado ao OpenNLP. Vou seguir os princípios SOLID e as melhores práticas do Spring Boot, criando uma arquitetura robusta e manutenível.

## Estrutura do Módulo DocExtractor

Primeiro, vamos visualizar a arquitetura completa do módulo:

```txt
docextractor/
├── domain/
│   ├── entity/
│   │   ├── ExtractedDocument.java
│   │   └── ExtractionMetadata.java
│   ├── enums/
│   │   ├── DocumentType.java
│   │   ├── ExtractionStatus.java
│   │   └── ExtractionMethod.java
│   └── valueobject/
│       ├── DocumentContent.java
│       └── QualityMetrics.java
├── dto/
│   ├── request/
│   │   └── DocumentExtractionRequest.java
│   ├── response/
│   │   ├── DocumentExtractionResponse.java
│   │   └── ExtractionResultDto.java
│   └── mapper/
│       └── DocumentMapper.java
├── service/
│   ├── DocumentExtractionService.java
│   ├── impl/
│   │   └── DocumentExtractionServiceImpl.java
│   ├── extractor/
│   │   ├── TextExtractor.java
│   │   ├── PdfTextExtractor.java
│   │   └── OcrTextExtractor.java
│   └── processor/
│       ├── TextProcessor.java
│       ├── TextNormalizer.java
│       └── TextQualityAnalyzer.java
├── factory/
│   ├── ExtractorFactory.java
│   └── ProcessorChainFactory.java
├── strategy/
│   ├── ExtractionStrategy.java
│   ├── NativeTextStrategy.java
│   ├── OcrStrategy.java
│   └── HybridStrategy.java
├── exception/
│   ├── DocumentExtractionException.java
│   ├── InvalidDocumentException.java
│   ├── ExtractionTimeoutException.java
│   └── UnsupportedDocumentTypeException.java
├── config/
│   ├── DocExtractorConfig.java
│   └── TesseractConfig.java
├── controller/
│   └── DocumentExtractionController.java
└── repository/
    └── ExtractedDocumentRepository.java
```

Agora vamos implementar cada componente seguindo os princípios SOLID:

## 1. Entidades de Domínio

### ExtractedDocument.java

```java
package br.edu.ifms.gad.docextractor.domain.entity;

import jakarta.persistence.*;
import lombok.*;
import java.time.LocalDateTime;
import java.util.UUID;

@Entity
@Table(name = "extracted_documents")
@Getter
@Setter
@Builder
@NoArgsConstructor
@AllArgsConstructor
public class ExtractedDocument {
    
    @Id
    private UUID id;
    
    @Column(name = "original_filename", nullable = false)
    private String originalFilename;
    
    @Column(name = "file_size")
    private Long fileSize;
    
    @Column(name = "mime_type")
    private String mimeType;
    
    @Lob
    @Column(name = "raw_text", columnDefinition = "TEXT")
    private String rawText;
    
    @Lob
    @Column(name = "processed_text", columnDefinition = "TEXT")
    private String processedText;
    
    @Enumerated(EnumType.STRING)
    @Column(name = "extraction_method")
    private ExtractionMethod extractionMethod;
    
    @Enumerated(EnumType.STRING)
    @Column(name = "status")
    private ExtractionStatus status;
    
    @Column(name = "quality_score")
    private Double qualityScore;
    
    @Column(name = "page_count")
    private Integer pageCount;
    
    @Column(name = "extraction_duration_ms")
    private Long extractionDurationMs;
    
    @Column(name = "error_message")
    private String errorMessage;
    
    @Column(name = "created_at")
    private LocalDateTime createdAt;
    
    @Column(name = "updated_at")
    private LocalDateTime updatedAt;
    
    @OneToOne(cascade = CascadeType.ALL, fetch = FetchType.LAZY)
    @JoinColumn(name = "metadata_id")
    private ExtractionMetadata metadata;
    
    @PrePersist
    protected void onCreate() {
        if (id == null) {
            id = UUID.randomUUID();
        }
        createdAt = LocalDateTime.now();
        updatedAt = LocalDateTime.now();
    }
    
    @PreUpdate
    protected void onUpdate() {
        updatedAt = LocalDateTime.now();
    }
}
```

### ExtractionMetadata.java

```java
package br.edu.ifms.gad.docextractor.domain.entity;

import jakarta.persistence.*;
import lombok.*;
import java.util.Map;
import java.util.UUID;

@Entity
@Table(name = "extraction_metadata")
@Getter
@Setter
@Builder
@NoArgsConstructor
@AllArgsConstructor
public class ExtractionMetadata {
    
    @Id
    private UUID id;
    
    @Column(name = "has_images")
    private Boolean hasImages;
    
    @Column(name = "has_tables")
    private Boolean hasTables;
    
    @Column(name = "has_forms")
    private Boolean hasForms;
    
    @Column(name = "encoding_detected")
    private String encodingDetected;
    
    @Column(name = "language_detected")
    private String languageDetected;
    
    @Column(name = "ocr_confidence")
    private Double ocrConfidence;
    
    @ElementCollection
    @CollectionTable(name = "extraction_warnings")
    @Column(name = "warning")
    private List<String> warnings;
    
    @ElementCollection
    @CollectionTable(name = "extraction_properties")
    @MapKeyColumn(name = "property_key")
    @Column(name = "property_value")
    private Map<String, String> additionalProperties;
    
    @PrePersist
    protected void onCreate() {
        if (id == null) {
            id = UUID.randomUUID();
        }
    }
}
```

## 2. Enums

### DocumentType.java

```java
package br.edu.ifms.gad.docextractor.domain.enums;

public enum DocumentType {
    PDF("application/pdf"),
    IMAGE("image/*"),
    SCANNED_PDF("application/pdf");
    
    private final String mimeType;
    
    DocumentType(String mimeType) {
        this.mimeType = mimeType;
    }
    
    public String getMimeType() {
        return mimeType;
    }
}
```

### ExtractionStatus.java

```java
package br.edu.ifms.gad.docextractor.domain.enums;

public enum ExtractionStatus {
    PENDING,
    PROCESSING,
    COMPLETED,
    FAILED,
    PARTIALLY_COMPLETED
}
```

### ExtractionMethod.java

```java
package br.edu.ifms.gad.docextractor.domain.enums;

public enum ExtractionMethod {
    NATIVE_TEXT,    // PDF com texto nativo
    OCR,            // Tesseract OCR
    HYBRID,         // Combinação de métodos
    MANUAL          // Fallback manual
}
```

## 3. Value Objects

### DocumentContent.java

```java
package br.edu.ifms.gad.docextractor.domain.valueobject;

import lombok.Value;
import java.util.List;
import java.util.Map;

@Value
public class DocumentContent {
    String rawText;
    String processedText;
    List<String> pages;
    Map<Integer, String> pageContent;
    QualityMetrics qualityMetrics;
    
    public boolean isHighQuality() {
        return qualityMetrics != null && 
               qualityMetrics.getOverallScore() > 0.7;
    }
    
    public boolean requiresOcr() {
        return rawText == null || rawText.trim().isEmpty() ||
               (qualityMetrics != null && qualityMetrics.getTextDensity() < 0.1);
    }
}
```

### QualityMetrics.java

```java
package br.edu.ifms.gad.docextractor.domain.valueobject;

import lombok.Builder;
import lombok.Value;

@Value
@Builder
public class QualityMetrics {
    Double overallScore;        // 0.0 a 1.0
    Double textDensity;         // Proporção de texto vs ruído
    Double readabilityScore;    // Baseado em métricas de legibilidade
    Integer characterCount;
    Integer wordCount;
    Integer lineCount;
    Double averageWordLength;
    Boolean hasSpecialCharacters;
    Boolean hasConsistentFormatting;
    
    public static QualityMetrics calculate(String text) {
        if (text == null || text.isEmpty()) {
            return QualityMetrics.builder()
                .overallScore(0.0)
                .textDensity(0.0)
                .readabilityScore(0.0)
                .characterCount(0)
                .wordCount(0)
                .lineCount(0)
                .averageWordLength(0.0)
                .hasSpecialCharacters(false)
                .hasConsistentFormatting(false)
                .build();
        }
        
        // Implementação dos cálculos
        String[] words = text.split("\\s+");
        String[] lines = text.split("\\n");
        
        int charCount = text.length();
        int wordCount = words.length;
        int lineCount = lines.length;
        
        double avgWordLength = words.length > 0 ? 
            text.replaceAll("\\s+", "").length() / (double) words.length : 0;
            
        boolean hasSpecialChars = text.matches(".*[^\\p{L}\\p{N}\\p{P}\\s].*");
        
        // Cálculo simplificado de densidade de texto
        double textDensity = calculateTextDensity(text);
        
        // Score de legibilidade baseado em múltiplos fatores
        double readability = calculateReadability(text, words, lines);
        
        // Score geral
        double overall = (textDensity * 0.4) + (readability * 0.6);
        
        return QualityMetrics.builder()
            .overallScore(overall)
            .textDensity(textDensity)
            .readabilityScore(readability)
            .characterCount(charCount)
            .wordCount(wordCount)
            .lineCount(lineCount)
            .averageWordLength(avgWordLength)
            .hasSpecialCharacters(hasSpecialChars)
            .hasConsistentFormatting(checkFormatting(text))
            .build();
    }
    
    private static double calculateTextDensity(String text) {
        // Remove espaços extras e caracteres especiais
        String cleanText = text.replaceAll("[^\\p{L}\\p{N}]", "");
        return cleanText.length() / (double) text.length();
    }
    
    private static double calculateReadability(String text, String[] words, String[] lines) {
        // Implementação simplificada do índice de legibilidade
        double avgWordsPerLine = words.length / (double) Math.max(1, lines.length);
        double avgCharsPerWord = text.length() / (double) Math.max(1, words.length);
        
        // Normalização para score entre 0 e 1
        double score = 1.0;
        if (avgWordsPerLine > 20) score -= 0.2;
        if (avgCharsPerWord > 10) score -= 0.2;
        if (avgCharsPerWord < 3) score -= 0.3;
        
        return Math.max(0, Math.min(1, score));
    }
    
    private static boolean checkFormatting(String text) {
        // Verifica se há padrões consistentes de formatação
        return text.contains("\n") && 
               !text.matches(".*\\n{3,}.*") && // Não tem muitas quebras consecutivas
               text.matches(".*[.!?]\\s*\\n.*"); // Tem pontuação antes de quebras
    }
}
```

## 4. DTOs e Mapper

### DocumentExtractionRequest.java

```java
package br.edu.ifms.gad.docextractor.dto.request;

import lombok.Builder;
import lombok.Data;
import org.springframework.web.multipart.MultipartFile;
import jakarta.validation.constraints.NotNull;

@Data
@Builder
public class DocumentExtractionRequest {
    
    @NotNull(message = "O arquivo é obrigatório")
    private MultipartFile file;
    
    private Boolean forceOcr;
    private String language;
    private Integer maxPages;
    private Boolean enhanceQuality;
}
```

### DocumentExtractionResponse.java

```java
package br.edu.ifms.gad.docextractor.dto.response;

import br.edu.ifms.gad.docextractor.domain.enums.ExtractionMethod;
import br.edu.ifms.gad.docextractor.domain.enums.ExtractionStatus;
import com.fasterxml.jackson.annotation.JsonInclude;
import lombok.Builder;
import lombok.Data;
import java.time.LocalDateTime;
import java.util.List;
import java.util.UUID;

@Data
@Builder
@JsonInclude(JsonInclude.Include.NON_NULL)
public class DocumentExtractionResponse {
    private UUID documentId;
    private String filename;
    private ExtractionStatus status;
    private ExtractionMethod method;
    private String extractedText;
    private Double qualityScore;
    private Integer pageCount;
    private Long processingTimeMs;
    private List<String> warnings;
    private LocalDateTime processedAt;
    private ExtractionMetrics metrics;
    
    @Data
    @Builder
    public static class ExtractionMetrics {
        private Integer characterCount;
        private Integer wordCount;
        private Integer lineCount;
        private Double textDensity;
        private Double readabilityScore;
    }
}
```

### DocumentMapper.java

```java
package br.edu.ifms.gad.docextractor.dto.mapper;

import br.edu.ifms.gad.docextractor.domain.entity.ExtractedDocument;
import br.edu.ifms.gad.docextractor.dto.response.DocumentExtractionResponse;
import org.springframework.stereotype.Component;

@Component
public class DocumentMapper {
    
    public DocumentExtractionResponse toResponse(ExtractedDocument document) {
        if (document == null) {
            return null;
        }
        
        DocumentExtractionResponse.ExtractionMetrics metrics = null;
        if (document.getMetadata() != null) {
            // Construir métricas a partir dos metadados
            metrics = DocumentExtractionResponse.ExtractionMetrics.builder()
                .characterCount(document.getProcessedText() != null ? 
                    document.getProcessedText().length() : 0)
                .wordCount(document.getProcessedText() != null ? 
                    document.getProcessedText().split("\\s+").length : 0)
                .lineCount(document.getProcessedText() != null ? 
                    document.getProcessedText().split("\\n").length : 0)
                .build();
        }
        
        return DocumentExtractionResponse.builder()
            .documentId(document.getId())
            .filename(document.getOriginalFilename())
            .status(document.getStatus())
            .method(document.getExtractionMethod())
            .extractedText(document.getProcessedText())
            .qualityScore(document.getQualityScore())
            .pageCount(document.getPageCount())
            .processingTimeMs(document.getExtractionDurationMs())
            .warnings(document.getMetadata() != null ? 
                document.getMetadata().getWarnings() : null)
            .processedAt(document.getUpdatedAt())
            .metrics(metrics)
            .build();
    }
}
```

## 5. Estratégias de Extração

### ExtractionStrategy.java

```java
package br.edu.ifms.gad.docextractor.strategy;

import br.edu.ifms.gad.docextractor.domain.valueobject.DocumentContent;
import java.io.InputStream;

public interface ExtractionStrategy {
    DocumentContent extract(InputStream inputStream, String filename);
    boolean supports(String mimeType, byte[] fileHeader);
    int getPriority();
}
```

### NativeTextStrategy.java

```java
package br.edu.ifms.gad.docextractor.strategy;

import br.edu.ifms.gad.docextractor.domain.valueobject.DocumentContent;
import br.edu.ifms.gad.docextractor.domain.valueobject.QualityMetrics;
import org.apache.pdfbox.pdmodel.PDDocument;
import org.apache.pdfbox.text.PDFTextStripper;
import org.springframework.stereotype.Component;
import lombok.extern.slf4j.Slf4j;

import java.io.InputStream;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

@Slf4j
@Component
public class NativeTextStrategy implements ExtractionStrategy {
    
    @Override
    public DocumentContent extract(InputStream inputStream, String filename) {
        try (PDDocument document = PDDocument.load(inputStream)) {
            PDFTextStripper stripper = new PDFTextStripper();
            
            // Extrai texto completo
            String fullText = stripper.getText(document);
            
            // Extrai texto por página
            List<String> pages = new ArrayList<>();
            Map<Integer, String> pageContent = new HashMap<>();
            
            for (int i = 1; i <= document.getNumberOfPages(); i++) {
                stripper.setStartPage(i);
                stripper.setEndPage(i);
                String pageText = stripper.getText(document);
                pages.add(pageText);
                pageContent.put(i, pageText);
            }
            
            // Calcula métricas de qualidade
            QualityMetrics metrics = QualityMetrics.calculate(fullText);
            
            log.info("Extração nativa concluída para {}: {} páginas, {} caracteres", 
                filename, pages.size(), fullText.length());
            
            return new DocumentContent(
                fullText,
                fullText, // Será processado depois
                pages,
                pageContent,
                metrics
            );
            
        } catch (Exception e) {
            log.error("Erro na extração nativa de texto: {}", e.getMessage());
            throw new RuntimeException("Falha na extração de texto nativo", e);
        }
    }
    
    @Override
    public boolean supports(String mimeType, byte[] fileHeader) {
        // Verifica se é PDF verificando o header
        return mimeType.equals("application/pdf") && 
               fileHeader != null && 
               fileHeader.length >= 4 &&
               fileHeader[0] == '%' && 
               fileHeader[1] == 'P' && 
               fileHeader[2] == 'D' && 
               fileHeader[3] == 'F';
    }
    
    @Override
    public int getPriority() {
        return 100; // Alta prioridade
    }
}
```

### OcrStrategy.java

```java
package br.edu.ifms.gad.docextractor.strategy;

import br.edu.ifms.gad.docextractor.domain.valueobject.DocumentContent;
import br.edu.ifms.gad.docextractor.domain.valueobject.QualityMetrics;
import net.sourceforge.tess4j.Tesseract;
import net.sourceforge.tess4j.TesseractException;
import org.apache.pdfbox.pdmodel.PDDocument;
import org.apache.pdfbox.rendering.ImageType;
import org.apache.pdfbox.rendering.PDFRenderer;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;
import lombok.extern.slf4j.Slf4j;

import javax.imageio.ImageIO;
import java.awt.image.BufferedImage;
import java.io.ByteArrayInputStream;
import java.io.InputStream;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

@Slf4j
@Component
public class OcrStrategy implements ExtractionStrategy {
    
    private final Tesseract tesseract;
    
    public OcrStrategy(@Value("${tesseract.data.path}") String dataPath) {
        this.tesseract = new Tesseract();
        this.tesseract.setDatapath(dataPath);
        this.tesseract.setLanguage("por"); // Português
        this.tesseract.setPageSegMode(1); // Automatic page segmentation with OSD
        this.tesseract.setOcrEngineMode(1); // Neural nets LSTM engine
    }
    
    @Override
    public DocumentContent extract(InputStream inputStream, String filename) {
        try {
            List<String> pages = new ArrayList<>();
            Map<Integer, String> pageContent = new HashMap<>();
            StringBuilder fullText = new StringBuilder();
            
            // Verifica se é PDF ou imagem
            byte[] bytes = inputStream.readAllBytes();
            
            if (isPdf(bytes)) {
                extractFromPdf(new ByteArrayInputStream(bytes), pages, pageContent, fullText);
            } else {
                extractFromImage(new ByteArrayInputStream(bytes), pages, pageContent, fullText);
            }
            
            String text = fullText.toString();
            QualityMetrics metrics = QualityMetrics.calculate(text);
            
            log.info("OCR concluído para {}: {} páginas extraídas", 
                filename, pages.size());
            
            return new DocumentContent(
                text,
                text, // Será processado depois
                pages,
                pageContent,
                metrics
            );
            
        } catch (Exception e) {
            log.error("Erro no OCR: {}", e.getMessage());
            throw new RuntimeException("Falha no processamento OCR", e);
        }
    }
    
    private void extractFromPdf(InputStream pdfStream, List<String> pages, 
                               Map<Integer, String> pageContent, 
                               StringBuilder fullText) throws Exception {
        
        try (PDDocument document = PDDocument.load(pdfStream)) {
            PDFRenderer renderer = new PDFRenderer(document);
            
            for (int i = 0; i < document.getNumberOfPages(); i++) {
                BufferedImage image = renderer.renderImageWithDPI(i, 300, ImageType.RGB);
                String pageText = tesseract.doOCR(image);
                
                pages.add(pageText);
                pageContent.put(i + 1, pageText);
                fullText.append(pageText).append("\n\n");
                
                log.debug("Página {} processada via OCR", i + 1);
            }
        }
    }
    
    private void extractFromImage(InputStream imageStream, List<String> pages,
                                 Map<Integer, String> pageContent, 
                                 StringBuilder fullText) throws Exception {
        
        BufferedImage image = ImageIO.read(imageStream);
        String text = tesseract.doOCR(image);
        
        pages.add(text);
        pageContent.put(1, text);
        fullText.append(text);
    }
    
    private boolean isPdf(byte[] bytes) {
        return bytes.length >= 4 &&
               bytes[0] == '%' && 
               bytes[1] == 'P' && 
               bytes[2] == 'D' && 
               bytes[3] == 'F';
    }
    
    @Override
    public boolean supports(String mimeType, byte[] fileHeader) {
        return mimeType.startsWith("image/") || 
               (mimeType.equals("application/pdf") && requiresOcr(fileHeader));
    }
    
    private boolean requiresOcr(byte[] fileHeader) {
        // Lógica para detectar se o PDF precisa de OCR
        // Por enquanto, retorna false (será melhorado)
        return false;
    }
    
    @Override
    public int getPriority() {
        return 50; // Prioridade média
    }
}
```

## 6. Processadores de Texto

### TextProcessor.java

```java
package br.edu.ifms.gad.docextractor.service.processor;

public interface TextProcessor {
    String process(String text);
    int getOrder();
}
```

### TextNormalizer.java

```java
package br.edu.ifms.gad.docextractor.service.processor;

import org.springframework.stereotype.Component;
import java.text.Normalizer;
import java.util.regex.Pattern;

@Component
public class TextNormalizer implements TextProcessor {
    
    private static final Pattern MULTIPLE_SPACES = Pattern.compile("\\s{2,}");
    private static final Pattern MULTIPLE_NEWLINES = Pattern.compile("\\n{3,}");
    private static final Pattern CONTROL_CHARS = Pattern.compile("[\\p{Cntrl}&&[^\\n\\r\\t]]");
    private static final Pattern BROKEN_WORDS = Pattern.compile("(\\w+)-\\s*\\n\\s*(\\w+)");
    
    @Override
    public String process(String text) {
        if (text == null || text.isEmpty()) {
            return text;
        }
        
        // 1. Normaliza encoding para UTF-8
        text = Normalizer.normalize(text, Normalizer.Form.NFC);
        
        // 2. Remove caracteres de controle (exceto \n, \r, \t)
        text = CONTROL_CHARS.matcher(text).replaceAll("");
        
        // 3. Corrige palavras quebradas por hifenização
        text = BROKEN_WORDS.matcher(text).replaceAll("$1$2");
        
        // 4. Normaliza espaços múltiplos
        text = MULTIPLE_SPACES.matcher(text).replaceAll(" ");
        
        // 5. Normaliza quebras de linha excessivas
        text = MULTIPLE_NEWLINES.matcher(text).replaceAll("\n\n");
        
        // 6. Remove espaços no início e fim de cada linha
        text = text.lines()
            .map(String::trim)
            .reduce((a, b) -> a + "\n" + b)
            .orElse("");
        
        // 7. Corrige problemas comuns de OCR
        text = fixCommonOcrErrors(text);
        
        return text.trim();
    }
    
    private String fixCommonOcrErrors(String text) {
        // Correções específicas para erros comuns de OCR em português
        return text
            .replace("ã0", "ão")
            .replace("ç@", "ça")
            .replace("||", "ll")
            .replace("l!", "li")
            .replace("0", "o") // Cuidado: pode ser agressivo demais
            .replaceAll("([a-z])1([a-z])", "$1i$2") // l1nha -> linha
            .replaceAll("\\b1(?=[a-z])", "i") // 1nício -> início
            .replaceAll("(?<=[a-z])1\\b", "l"); // fina1 -> final
    }
    
    @Override
    public int getOrder() {
        return 1; // Primeira etapa do processamento
    }
}
```

### TextQualityAnalyzer.java

```java
package br.edu.ifms.gad.docextractor.service.processor;

import br.edu.ifms.gad.docextractor.domain.valueobject.QualityMetrics;
import org.springframework.stereotype.Component;
import lombok.extern.slf4j.Slf4j;

@Slf4j
@Component
public class TextQualityAnalyzer implements TextProcessor {
    
    private static final double MIN_QUALITY_THRESHOLD = 0.5;
    
    @Override
    public String process(String text) {
        QualityMetrics metrics = QualityMetrics.calculate(text);
        
        log.info("Análise de qualidade: Score={}, Densidade={}, Legibilidade={}", 
            metrics.getOverallScore(), 
            metrics.getTextDensity(), 
            metrics.getReadabilityScore());
        
        if (metrics.getOverallScore() < MIN_QUALITY_THRESHOLD) {
            log.warn("Texto de baixa qualidade detectado. Score: {}", 
                metrics.getOverallScore());
            
            // Aqui você pode adicionar lógica para melhorar o texto
            // ou marcar para revisão manual
        }
        
        // Este processor não modifica o texto, apenas analisa
        return text;
    }
    
    @Override
    public int getOrder() {
        return 99; // Última etapa - apenas análise
    }
}
```

## 7. Factory Pattern

### ExtractorFactory.java

```java
package br.edu.ifms.gad.docextractor.factory;

import br.edu.ifms.gad.docextractor.strategy.ExtractionStrategy;
import org.springframework.stereotype.Component;
import java.util.List;
import java.util.Comparator;

@Component
public class ExtractorFactory {
    
    private final List<ExtractionStrategy> strategies;
    
    public ExtractorFactory(List<ExtractionStrategy> strategies) {
        // Ordena estratégias por prioridade
        this.strategies = strategies.stream()
            .sorted(Comparator.comparing(ExtractionStrategy::getPriority).reversed())
            .toList();
    }
    
    public ExtractionStrategy getStrategy(String mimeType, byte[] fileHeader) {
        return strategies.stream()
            .filter(strategy -> strategy.supports(mimeType, fileHeader))
            .findFirst()
            .orElseThrow(() -> new UnsupportedOperationException(
                "Nenhuma estratégia disponível para o tipo: " + mimeType));
    }
}
```

### ProcessorChainFactory.java

```java
package br.edu.ifms.gad.docextractor.factory;

import br.edu.ifms.gad.docextractor.service.processor.TextProcessor;
import org.springframework.stereotype.Component;
import java.util.List;
import java.util.Comparator;

@Component
public class ProcessorChainFactory {
    
    private final List<TextProcessor> processors;
    
    public ProcessorChainFactory(List<TextProcessor> processors) {
        // Ordena processadores por ordem de execução
        this.processors = processors.stream()
            .sorted(Comparator.comparing(TextProcessor::getOrder))
            .toList();
    }
    
    public String processText(String text) {
        String result = text;
        for (TextProcessor processor : processors) {
            result = processor.process(result);
        }
        return result;
    }
}
```

## 8. Service Layer

### DocumentExtractionService.java

```java
package br.edu.ifms.gad.docextractor.service;

import br.edu.ifms.gad.docextractor.dto.request.DocumentExtractionRequest;
import br.edu.ifms.gad.docextractor.dto.response.DocumentExtractionResponse;
import java.util.UUID;

public interface DocumentExtractionService {
    DocumentExtractionResponse extractText(DocumentExtractionRequest request);
    DocumentExtractionResponse getExtractionResult(UUID documentId);
    void retryExtraction(UUID documentId);
}
```

### DocumentExtractionServiceImpl.java

```java
package br.edu.ifms.gad.docextractor.service.impl;

import br.edu.ifms.gad.docextractor.domain.entity.ExtractedDocument;
import br.edu.ifms.gad.docextractor.domain.entity.ExtractionMetadata;
import br.edu.ifms.gad.docextractor.domain.enums.ExtractionStatus;
import br.edu.ifms.gad.docextractor.domain.valueobject.DocumentContent;
import br.edu.ifms.gad.docextractor.dto.mapper.DocumentMapper;
import br.edu.ifms.gad.docextractor.dto.request.DocumentExtractionRequest;
import br.edu.ifms.gad.docextractor.dto.response.DocumentExtractionResponse;
import br.edu.ifms.gad.docextractor.exception.DocumentExtractionException;
import br.edu.ifms.gad.docextractor.exception.InvalidDocumentException;
import br.edu.ifms.gad.docextractor.factory.ExtractorFactory;
import br.edu.ifms.gad.docextractor.factory.ProcessorChainFactory;
import br.edu.ifms.gad.docextractor.repository.ExtractedDocumentRepository;
import br.edu.ifms.gad.docextractor.service.DocumentExtractionService;
import br.edu.ifms.gad.docextractor.strategy.ExtractionStrategy;
import lombok.RequiredArgsConstructor;
import lombok.extern.slf4j.Slf4j;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;
import org.springframework.web.multipart.MultipartFile;

import java.io.IOException;
import java.util.ArrayList;
import java.util.UUID;

@Slf4j
@Service
@RequiredArgsConstructor
public class DocumentExtractionServiceImpl implements DocumentExtractionService {
    
    private final ExtractedDocumentRepository repository;
    private final ExtractorFactory extractorFactory;
    private final ProcessorChainFactory processorChainFactory;
    private final DocumentMapper documentMapper;
    
    private static final long MAX_FILE_SIZE = 50 * 1024 * 1024; // 50MB
    
    @Override
    @Transactional
    public DocumentExtractionResponse extractText(DocumentExtractionRequest request) {
        validateRequest(request);
        
        MultipartFile file = request.getFile();
        long startTime = System.currentTimeMillis();
        
        try {
            // 1. Cria entidade para rastreamento
            ExtractedDocument document = createDocument(file);
            document = repository.save(document);
            
            // 2. Obtém estratégia apropriada
            byte[] fileHeader = getFileHeader(file);
            ExtractionStrategy strategy = extractorFactory.getStrategy(
                file.getContentType(), fileHeader);
            
            // 3. Extrai conteúdo
            DocumentContent content = strategy.extract(
                file.getInputStream(), file.getOriginalFilename());
            
            // 4. Processa o texto
            String processedText = processorChainFactory.processText(
                content.getProcessedText());
            
            // 5. Atualiza documento com resultados
            updateDocumentWithResults(document, content, processedText, startTime);
            
            // 6. Salva e retorna
            document = repository.save(document);
            return documentMapper.toResponse(document);
            
        } catch (Exception e) {
            log.error("Erro na extração de texto: {}", e.getMessage(), e);
            throw new DocumentExtractionException(
                "Falha na extração de texto: " + e.getMessage(), e);
        }
    }
    
    @Override
    public DocumentExtractionResponse getExtractionResult(UUID documentId) {
        ExtractedDocument document = repository.findById(documentId)
            .orElseThrow(() -> new DocumentExtractionException(
                "Documento não encontrado: " + documentId));
        
        return documentMapper.toResponse(document);
    }
    
    @Override
    @Transactional
    public void retryExtraction(UUID documentId) {
        ExtractedDocument document = repository.findById(documentId)
            .orElseThrow(() -> new DocumentExtractionException(
                "Documento não encontrado: " + documentId));
        
        if (document.getStatus() != ExtractionStatus.FAILED) {
            throw new DocumentExtractionException(
                "Apenas documentos com falha podem ser reprocessados");
        }
        
        // Implementar lógica de retry
        log.info("Reprocessando documento: {}", documentId);
        // ... código de reprocessamento
    }
    
    private void validateRequest(DocumentExtractionRequest request) {
        if (request.getFile() == null || request.getFile().isEmpty()) {
            throw new InvalidDocumentException("Arquivo não fornecido");
        }
        
        if (request.getFile().getSize() > MAX_FILE_SIZE) {
            throw new InvalidDocumentException(
                "Arquivo excede o tamanho máximo permitido de 50MB");
        }
        
        String contentType = request.getFile().getContentType();
        if (contentType == null || 
            (!contentType.startsWith("application/pdf") && 
             !contentType.startsWith("image/"))) {
            throw new InvalidDocumentException(
                "Tipo de arquivo não suportado: " + contentType);
        }
    }
    
    private ExtractedDocument createDocument(MultipartFile file) {
        return ExtractedDocument.builder()
            .originalFilename(file.getOriginalFilename())
            .fileSize(file.getSize())
            .mimeType(file.getContentType())
            .status(ExtractionStatus.PROCESSING)
            .build();
    }
    
    private byte[] getFileHeader(MultipartFile file) throws IOException {
        byte[] header = new byte[Math.min(1024, (int) file.getSize())];
        file.getInputStream().read(header);
        return header;
    }
    
    private void updateDocumentWithResults(ExtractedDocument document, 
                                         DocumentContent content, 
                                         String processedText,
                                         long startTime) {
        document.setRawText(content.getRawText());
        document.setProcessedText(processedText);
        document.setQualityScore(content.getQualityMetrics().getOverallScore());
        document.setPageCount(content.getPages().size());
        document.setExtractionDurationMs(System.currentTimeMillis() - startTime);
        document.setStatus(ExtractionStatus.COMPLETED);
        
        // Criar e configurar metadados
        ExtractionMetadata metadata = ExtractionMetadata.builder()
            .hasImages(false) // Implementar detecção
            .hasTables(false) // Implementar detecção
            .languageDetected("pt-BR")
            .encodingDetected("UTF-8")
            .warnings(new ArrayList<>())
            .build();
        
        if (content.getQualityMetrics().getOverallScore() < 0.5) {
            metadata.getWarnings().add("Qualidade de texto baixa detectada");
        }
        
        document.setMetadata(metadata);
    }
}
```

## 9. Exception Handling

### DocumentExtractionException.java

```java
package br.edu.ifms.gad.docextractor.exception;

public class DocumentExtractionException extends RuntimeException {
    public DocumentExtractionException(String message) {
        super(message);
    }
    
    public DocumentExtractionException(String message, Throwable cause) {
        super(message, cause);
    }
}
```

### InvalidDocumentException.java

```java
package br.edu.ifms.gad.docextractor.exception;

public class InvalidDocumentException extends DocumentExtractionException {
    public InvalidDocumentException(String message) {
        super(message);
    }
}
```

## 10. Controller

### DocumentExtractionController.java

```java
package br.edu.ifms.gad.docextractor.controller;

import br.edu.ifms.gad.docextractor.dto.request.DocumentExtractionRequest;
import br.edu.ifms.gad.docextractor.dto.response.DocumentExtractionResponse;
import br.edu.ifms.gad.docextractor.service.DocumentExtractionService;
import io.swagger.v3.oas.annotations.Operation;
import io.swagger.v3.oas.annotations.tags.Tag;
import lombok.RequiredArgsConstructor;
import org.springframework.http.MediaType;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;
import org.springframework.web.multipart.MultipartFile;

import java.util.UUID;

@RestController
@RequestMapping("/api/v1/documents/extract")
@RequiredArgsConstructor
@Tag(name = "Document Extraction", description = "API para extração de texto de documentos")
public class DocumentExtractionController {
    
    private final DocumentExtractionService extractionService;
    
    @PostMapping(consumes = MediaType.MULTIPART_FORM_DATA_VALUE)
    @Operation(summary = "Extrai texto de um documento PDF ou imagem")
    public ResponseEntity<DocumentExtractionResponse> extractText(
            @RequestParam("file") MultipartFile file,
            @RequestParam(value = "forceOcr", required = false) Boolean forceOcr,
            @RequestParam(value = "language", required = false, defaultValue = "por") String language) {
        
        DocumentExtractionRequest request = DocumentExtractionRequest.builder()
            .file(file)
            .forceOcr(forceOcr)
            .language(language)
            .build();
        
        DocumentExtractionResponse response = extractionService.extractText(request);
        return ResponseEntity.ok(response);
    }
    
    @GetMapping("/{documentId}")
    @Operation(summary = "Obtém resultado de uma extração")
    public ResponseEntity<DocumentExtractionResponse> getExtractionResult(
            @PathVariable UUID documentId) {
        
        DocumentExtractionResponse response = extractionService.getExtractionResult(documentId);
        return ResponseEntity.ok(response);
    }
    
    @PostMapping("/{documentId}/retry")
    @Operation(summary = "Reprocessa uma extração que falhou")
    public ResponseEntity<Void> retryExtraction(@PathVariable UUID documentId) {
        extractionService.retryExtraction(documentId);
        return ResponseEntity.accepted().build();
    }
}
```

## 11. Repository

### ExtractedDocumentRepository.java

```java
package br.edu.ifms.gad.docextractor.repository;

import br.edu.ifms.gad.docextractor.domain.entity.ExtractedDocument;
import br.edu.ifms.gad.docextractor.domain.enums.ExtractionStatus;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.jpa.repository.Query;
import org.springframework.stereotype.Repository;

import java.time.LocalDateTime;
import java.util.List;
import java.util.UUID;

@Repository
public interface ExtractedDocumentRepository extends JpaRepository<ExtractedDocument, UUID> {
    
    List<ExtractedDocument> findByStatus(ExtractionStatus status);
    
    @Query("SELECT e FROM ExtractedDocument e WHERE e.status = :status " +
           "AND e.createdAt < :createdBefore")
    List<ExtractedDocument> findStaleDocuments(ExtractionStatus status, 
                                              LocalDateTime createdBefore);
    
    @Query("SELECT AVG(e.qualityScore) FROM ExtractedDocument e " +
           "WHERE e.createdAt >= :startDate")
    Double getAverageQualityScore(LocalDateTime startDate);
}
```

## 12. Configuração

### DocExtractorConfig.java

```java
package br.edu.ifms.gad.docextractor.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.scheduling.annotation.EnableAsync;
import org.springframework.scheduling.concurrent.ThreadPoolTaskExecutor;

import java.util.concurrent.Executor;

@Configuration
@EnableAsync
public class DocExtractorConfig {
    
    @Bean(name = "documentProcessingExecutor")
    public Executor documentProcessingExecutor() {
        ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
        executor.setCorePoolSize(2);
        executor.setMaxPoolSize(5);
        executor.setQueueCapacity(100);
        executor.setThreadNamePrefix("DocExtractor-");
        executor.initialize();
        return executor;
    }
}
```

### TesseractConfig.java

```java
package br.edu.ifms.gad.docextractor.config;

import net.sourceforge.tess4j.Tesseract;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class TesseractConfig {
    
    @Value("${tesseract.data.path:/usr/share/tesseract-ocr/4.00/tessdata}")
    private String dataPath;
    
    @Bean
    public Tesseract tesseract() {
        Tesseract tesseract = new Tesseract();
        tesseract.setDatapath(dataPath);
        tesseract.setLanguage("por");
        tesseract.setPageSegMode(1);
        tesseract.setOcrEngineMode(1);
        return tesseract;
    }
}
```

## 13. Application Properties

```java
# application.yml
docextractor:
  tesseract:
    data-path: /usr/share/tesseract-ocr/4.00/tessdata
  processing:
    max-file-size: 52428800 # 50MB
    timeout-seconds: 300
  quality:
    min-threshold: 0.5
    ocr-confidence-threshold: 0.7
```

## Como Este Módulo se Integra ao GAD

Este módulo de extração fornece a base sólida que o OpenNLP precisa. O fluxo completo seria:

1.  **Upload do Certificado** → DocumentExtractionController
2.  **Extração Inteligente** → Escolhe entre PDF nativo ou OCR
3.  **Processamento de Texto** → Normalização e limpeza
4.  **Análise de Qualidade** → Garante texto utilizável
5.  **Saída Limpa** → Texto pronto para o OpenNLP

### Exemplo de Uso no Pipeline GAD:

```java
@Service
public class CertificateProcessingService {
    
    @Autowired
    private DocumentExtractionService extractionService;
    
    @Autowired
    private OpenNLPEntityExtractor entityExtractor;
    
    public CertificateValidationResult processCertificate(MultipartFile file) {
        // 1. Extrai texto usando nosso módulo robusto
        DocumentExtractionResponse extraction = extractionService.extractText(
            DocumentExtractionRequest.builder()
                .file(file)
                .build()
        );
        
        // 2. Verifica qualidade
        if (extraction.getQualityScore() < 0.7) {
            log.warn("Qualidade baixa: {}", extraction.getQualityScore());
            // Pode solicitar revisão manual
        }
        
        // 3. Envia texto limpo para OpenNLP
        CertificateEntities entities = entityExtractor.extract(
            extraction.getExtractedText()
        );
        
        // 4. Continua com classificação...
        return validate(entities);
    }
}
```
