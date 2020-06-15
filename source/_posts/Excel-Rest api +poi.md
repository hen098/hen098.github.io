---
title: "Excel-Rest API + poi"
tags: ["poi", "excel"]
--- 

## 엑셀 모듈 추가 
### build.gradle
```yml
// poi  
compile group: 'org.apache.poi', name: 'poi-ooxml', version: '3.9'
```

## 파일 다운로드 - 노출 헤더 설정 
`CONTENT_DISPOSITION`에 파일명과 확장자를 담아서 반환해야 하는데, 기본적으로 해당 헤더는 브라우저에서 접근이 불가능한 헤더이다. 

아래와 같은 설정을 해주어야 브라우저에서 접근이 가능하다. 

```java
@Bean  
public CorsConfigurationSource corsConfigurationSource() {  
    CorsConfiguration configuration = new CorsConfiguration();  
  configuration.addAllowedOrigin(mappingUrl);  
  configuration.addAllowedMethod("*");  
  configuration.addAllowedHeader("*");  
  configuration.setAllowCredentials(true);  
  configuration.setMaxAge(3600L);  
  
  configuration.setExposedHeaders(Arrays.asList("Content-Disposition"));  // 해당 헤더 노출 설정
  
  UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();  
  source.registerCorsConfiguration("/**", configuration);  
  
 return source;  
}
```

## Excel 기능 구현
-  데이터 > 엑셀 파일로 변환 
### Controller
- header에 `CONTENT_DISPOSITION`로 파일명.확장자 담음
```java
/**  
 * 지원자 정보 엑셀 다운로드  
  *  
 * @param condition  
  * @return  
  */  
@PostMapping("/excel/applicants")  
public ResponseEntity<InputStreamResource> applicantsExcelFileDown(@RequestBody ApplSearchCondition condition) {  
    FileDto excel = excelService.applicantsExcelFileDown(condition);  
  
 return ResponseEntity.ok()  
            .header(HttpHeaders.CONTENT_DISPOSITION, "attachment; filename=" + excel.getFileName() + ".xlsx")  
            .body(excel.getResource());  
}
```

- 엑셀 파일 > Map 변환 
	- Map을 다시 데이터(Object로 변환하고 싶었지만, 매핑하는 간단한 방법 못 찾음)

```java
public class ExcelGenerator {  
  
    public static <E> ByteArrayInputStream dataToExcel(String[] columns, List<E> dataList) {  
        try(  
                ByteArrayOutputStream out = new ByteArrayOutputStream();  
  ){  
            Workbook workbook = new XSSFWorkbook();  
  CreationHelper createHelper = workbook.getCreationHelper();  
  
  Sheet sheet = workbook.createSheet();  
  
  Font headerFont = workbook.createFont();  
//            headerFont.setColor(IndexedColors.BLUE.getIndex());   // Header 폰트 색 변경  
  headerFont.setFontName("맑은 고딕");  
  headerFont.setBoldweight(Font.BOLDWEIGHT_BOLD);  
  
  CellStyle headerCellStyle = workbook.createCellStyle();  
  headerCellStyle.setFont(headerFont);  
  // Background 색 채움  
  headerCellStyle.setFillForegroundColor(IndexedColors.GREY_25_PERCENT.getIndex());  
  headerCellStyle.setFillPattern(CellStyle.SOLID_FOREGROUND);  
  
  // Row for Header  
  Row headerRow = sheet.createRow(0);  
  
  // Header  
  for (int col = 0; col < columns.length; col++) {  
                Cell cell = headerRow.createCell(col);  
  cell.setCellValue(columns[col]);  
  cell.setCellStyle(headerCellStyle);  
  }  
  
            ObjectMapper oMapper = new ObjectMapper();  
  
 int rowIdx = 1;  
 for (E data : dataList) {  
                Row row = sheet.createRow(rowIdx++);  
  
  Map<String, Object> map = oMapper.convertValue(data, Map.class);  
  
 int columnIdx = 0;  
 for (String key : map.keySet()){  
                    row.createCell(columnIdx++).setCellValue(map.get(key).toString());  
  }  
            }  
  
            workbook.write(out);  
 return new ByteArrayInputStream(out.toByteArray());  
  } catch (IOException e) {  
            throw new ExcelIOException("엑셀 파일 생성을 실패했습니다.");  
  }  
    }  
  
    public static List<Map<String, Object>> excelToData(InputStream is) {  
        try{  
            Workbook workbook = new XSSFWorkbook(is);  
  
  Sheet sheet = workbook.getSheetAt(0);  
  Iterator<Row> rows = sheet.iterator();  
  List<Map<String, Object>> maps = new ArrayList<>();  
  
  // header  
  String[] header = getHeader(sheet.getRow(0));  
  
 int rowIdx = 0;  
 while (rows.hasNext()) {  
                Row currentRow = rows.next();  
  
  // skip header  
  if(rowIdx == 0) {  
                    rowIdx++;  
 continue;  }  
  
                Iterator<Cell> cellsInRow = currentRow.iterator();  
  Map<String, Object> map = new HashMap<>();  
  
 int cellIdx = 0;  
 while (cellsInRow.hasNext()) {  
                    Cell currentCell = cellsInRow.next();  
  
  map.put(header[cellIdx], currentCell.getStringCellValue());  
  cellIdx++;  
  }  
  
                maps.add(map);  
  }  
  
            return maps;  
  } catch (IOException e) {  
            throw new ExcelIOException("엑셀 파일 읽기를 실패했습니다.");  
  }  
    }  
  
    /**  
 * 엑셀 에서 헤더 추출  
  *  
 * @param row  
  * @return  
  */  
  private static String[] getHeader(Row row) {  
        List<String> headers = new ArrayList<>();  
 for(Cell cell : row) {  
            headers.add(cell.getStringCellValue());  
  }  
        return headers.toArray(new String[headers.size()]);  
  }  
  
}
```

참고 
[https://grokonez.com/spring-framework/spring-boot/excel-file-download-from-springboot-restapi-apache-poi-mysql](https://grokonez.com/spring-framework/spring-boot/excel-file-download-from-springboot-restapi-apache-poi-mysql)


[https://grokonez.com/spring-framework/spring-boot/excel-file-upload-download-using-apache-poi-springboot-restapis-spring-jpa-thymeleaf-to-mysql](https://grokonez.com/spring-framework/spring-boot/excel-file-upload-download-using-apache-poi-springboot-restapis-spring-jpa-thymeleaf-to-mysql)
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTczOTg4NjY5NSwtMTQ4NDM4NjE2NSwtMT
YwMTM0NTg4N119
-->