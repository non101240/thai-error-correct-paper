# BiDrectional Attention Encoder

## Encoding
error_token
    --> embedding -> LSTM -->
    encoded_error

context_token 
    --> embedding -> LSTM -->
    encoded_context

## Similarity
encoded_error, encoded_context
    --> matrix attention -->
    similarity

## Input to Context attention (aka Context2Query)
similarity
    --> softmax (attention on Context) -->
    input_context_attention (encoded_context selection for each input)

encoded_context, input_context_attention
    --> weighted sum -->
    input_context_vectors (encoded_context selected for each input)

## Context to Input attention (aka Query2Context)
similarity
    -->
        Fill in missing values ->
        max similarity of each encoded_error to any encoded_context ->
        softmax (attention on Error)
    -->
    context_input_attention (encoded_input selection from the context)

encoded_input, context_input_attention
    --> weighted sum -->
    context_input_vector (encoded_input selected from the context)
    --> expand -->atten\
    context_input_vectors (for each input, all the same vector)

## Output
merged_encoding
    == [
        encoded_error,
        input_context_vectors,
        encoded_error * input_context_vectors,
        encoded_error * context_input_vectors,
    ]

# Seq2Seq

forward(error_tokens, context_tokens)

    # Encoding

    ## Forward Enmbedder and Encoder
    encoder_outputs = encode(error_tokens, context_tokens)
        embedding = bidaf_embedder(error_tokens, context_tokens)    # BiDAF Encoder
        encoder_outputs = encoder(embedding)                        # GRU
        return encoder_outputs

    ## Obtain hidden state intialization from last step from encoder
    decoder_hidden = last_step(encoder_outputs)

    ## ???
    decoder_context = zeros(encoder_outputs.shape)


    # Decoding

    For steps
        attention_input_vector = prepare_output_projections(output_last_timestep)
            embedded_input = target_embedder(output_last_timestep)
            encoder_attention = attention(encoder_outputs, decoder_hidden)
            attention_input_vector = weighted_sum(encoder_outputs, encoder_attention)
            return embedded_input, attention_input

        decoder_input = concat(embedded_input, attention_input)

        decoder_hidden, decoder_cell_state = LSTM(decoder_input, decoder_hidden, decoder_cell_state)

        output_projection = output_projection_layer(decoder_hidden)

        output = softmax(output_projection)