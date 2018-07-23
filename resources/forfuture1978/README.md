> give a lot of thanks to forfuture1978 and javenstudio.

#### basic chain

    DocConsumer / DocConsumerPerThread
    --> code: DocFieldProcessor / DocFieldProcessorPerThread
    
          --> code: StoredFieldsWriter / StoredFieldsWriterPerThread / StoredFieldsWriterPerField
          
          --> DocFieldConsumer / DocFieldConsumerPerThread / DocFieldConsumerPerField
          
            --> code: DocFieldConsumers / DocFieldConsumersPerThread / DocFieldConsumersPerField
            
            --> code: DocInverter / DocInverterPerThread / DocInverterPerField
            
                --> InvertedDocEndConsumer / InvertedDocConsumerPerThread / InvertedDocConsumerPerField
                    --> code: NormsWriter / NormsWriterPerThread / NormsWriterPerField
                
                --> InvertedDocConsumer / InvertedDocConsumerPerThread / InvertedDocConsumerPerField
                    --> code: TermsHash / TermsHashPerThread / TermsHashPerField

                        --> TermsHashConsumer / TermsHashConsumerPerThread / TermsHashConsumerPerField
                          --> code: FreqProxTermsWriter / FreqProxTermsWriterPerThread / FreqProxTermsWriterPerField
                          --> code: TermVectorsTermsWriter / TermVectorsTermsWriterPerThread / TermVectorsTermsWriterPerField

    DocumentsWriter::
    
    static final IndexingChain defaultIndexingChain = new IndexingChain() 
    {
        @Override
        DocConsumer getChain(DocumentsWriter documentsWriter) 
        {
              final TermsHashConsumer termVectorsWriter = new TermVectorsTermsWriter(documentsWriter);
              final TermsHashConsumer freqProxWriter = new FreqProxTermsWriter();
            
              final InvertedDocConsumer  termsHash = new TermsHash(documentsWriter, true, freqProxWriter,
                                                                   new TermsHash(documentsWriter, false, termVectorsWriter, null));
              final NormsWriter normsWriter = new NormsWriter();
              
              final DocInverter docInverter = new DocInverter(termsHash, normsWriter);
              
              return new DocFieldProcessor(documentsWriter, docInverter);
        }
    };


#### thread chain



#### field chain
























