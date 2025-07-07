<div class="sticky-glossary">

**Glossary**

- **CPT** - Current Procedural Terminology codes maintained by the American Medical Association (AMA)

- **EMR** - Electronic Medical Record

</div>

### From Patient to Data: Clinical and Molecular Data Journey
It all begins with the need to identify the underlying genetic cause of a disease. I’ll use a made-up example to describe this journey. Patient Evelyn Reed has been diagnosed with IBD. 

### Performing the procedure
To confirm the diagnosis and finalize treatment strategies, the gastroenterologist orders a diagnostic procedure such as a colonoscopy with ileoscopy. This allows visualization of the entire large intestine and the terminal ileum.
??? info "For those interested in additional details, click to expand, else skip to [Specimen Collection](#specimen-collection)"
    Both the procedure order and the procedure performed are recorded in the EMR. Typical metadata captured include unique identifiers, dates, ordering and performing doctors, CPT codes, and free-text notes. These records are linked to the patient via the unique patient identifier.

### Specimen Collection
During the endoscopy, biopsies are taken from various segments of the large intestine, including both visibly affected and normal-looking areas. 
??? info ""For those interested in additional details, click to expand, else skip to [At Histopath Lab](at-histopath.md)"
    These samples are recorded as Specimen entries in the EMR, with metadata such as collection date, type, source location, container type, and volume. The specimen is linked to both the performed procedure and the patient.

[← Previous](index.md) | [Next →](at-histopath.md)