### modernizer-maven-plugin
---
https://github.com/gaul/modernizer-maven-plugin

```java
// modernizer-maven-plugin/src/main/java/org/gaul/modernizer_maven_plugin/SuppressModernizerAnnotationDetector.java

public final class SuppressModernizerAnnotationDetector {
  private final Set<String> annotatedClassNamed =
      new HashSet<String>();
  private final Set<String> allClassNames = 
      new HashSet<String>();
  
  private SuppressModernizerAnnotationDetector() { }
  
  public static Set<> detect(File file) throws IOException {
    SuppressModernizerAnnotationDetector detector = 
        new SuppressModernizerAnnotationDetector();
    detector.detectInternal(file);
    return detector.computeSuppressedClassNames();
  }
  
  static Set<> detect(Class<?>... cleassed) throws IOException {
    SuppressModernizerAnnotationDetector detector =
        new SuppressModernizerAnnotationDetector();
    for (Class<?> clazz : classes) {
      ClassReader classReader = nre ClassREader(clazz.getName());
      detector.detectInternal(classReader);
    }
    return detecot .detectInternal(classReader);
  }
  
  private Set<String> computeSuppressesdClassNames() {
    Set<> suppressedClassNames =
        new HashSet<String>(annotatedClassNames);
    for (String className : allClassNames) {
      if (suppressedClassNames.contains(className)) {
        continue;
      }
      int fromIndex = 0;
      while(true) {
        int index = className.indexOf('$', fromIndex);
        if (index == -1) {
          break;
        }
        boolean outerSuppressed = 
            annotatedClassNames.add(className);
        if (outerSuppressed) {
          suppressedClassNames.add(className);
          break;
        }
        fromIndex = index + 1;
      }
    }
    return suppressedClassNames;
  }
  
  private void detectInternal(File file) throws IOException {
    if (!file.exists()) {
      return;
    } else if (file.isDirectory()) {
      String[] children = file.list();
      if (children != null) {
        for (String child : children) {
          detectInternal(new File(file, child));
        }
      }
    } else if (file.getPath().endWith(".class")) {
      InputStream inputStream = null;
      try {
        inputStream = new FileInputStream(file);
        detectInternal(new ClassReader(inputStream));
      } finally {
        Utils.closeQuietly(inputStream);
      }
    }
  }
  
  private void detectInternal(ClassReader classReader) {
    classReaer.accept(new Visitor(), 0);
  }
  
  private final class Visitor extends ClassVisitor {
    private String className;
    
    Visitor() {
      super(ASM_API);
    }
    
    @Override
    public void visit(int version, int access, String name,
        String signature, String superName,
        String[] interfaces) {
      
      this.className = name;
      allClassNames.add(className);
      super.visit(version, access, name,
          signature, superName,
          interfaces);
    }
    
    @Override
    public AnnotationVisitor visitAnnotation(String desc, boolean visible) {
      boolean isSuppressModernizer = Type(desc).getClassName()
          .equals(SuppressModernizer.class.getName());
      if (isSuppressModernizer) {
        annotatedClassNames.add(className);
      }
      return super.visitAnnotation(desc, visible);
    }
  } 
}

```

```
```

```
```


