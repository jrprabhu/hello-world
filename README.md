# hello-world
Just another hello world git repository


import streamlit as st[span_2](start_span)[span_2](end_span)

from policy_engine import answer_question[span_3](start_span)[span_3](end_span)

st.set_page_config(page_title="PolicyPal", page_icon="📑", layout="wide")[span_4](start_span)[span_4](end_span)
st.markdown([span_5](start_span)[span_5](end_span)
    """
    <style>
        .stApp { background: #eef3f8; }
        h1 { color: #173c5b; }
        .source { border: 1px solid #b9d4ed; border-radius: 10px; padding: 12px; background: #f8fbfe; margin-top: 8px; }
        .source-title { color: #145da0; font-weight: 700; }
    </style>
    """,
    unsafe_allow_html=True,
)[span_6](start_span)[span_6](end_span)

st.title("PolicyPal")[span_7](start_span)[span_7](end_span)
st.caption("Synthetic Travel Policy Assistant | Offline capstone demo")[span_8](start_span)[span_8](end_span)

examples = {[span_9](start_span)[span_9](end_span)
    "Grounded answer": "What hotel cost can I claim for a trip to Singapore?",[span_10](start_span)[span_10](end_span)
    "Insufficient evidence": "Can I upgrade to business class for a personal extension?",[span_11](start_span)[span_11](end_span)
}[span_12](start_span)[span_12](end_span)
selected = st.selectbox("Demo scenario", list(examples))[span_13](start_span)[span_13](end_span)
question = st.text_input("Ask a travel-policy question", value=examples[selected])[span_14](start_span)[span_14](end_span)

if st.button("Ask PolicyPal", type="primary"):[span_15](start_span)[span_15](end_span)
    st.session_state.result = answer_question(question)[span_16](start_span)[span_16](end_span)
    st.session_state.question = question[span_17](start_span)[span_17](end_span)

if "result" not in st.session_state:[span_18](start_span)[span_18](end_span)[span_19](start_span)[span_19](end_span)
    st.session_state.result = answer_question(examples[selected])[span_20](start_span)[span_20](end_span)[span_21](start_span)[span_21](end_span)
    st.session_state.question = examples[selected][span_22](start_span)[span_22](end_span)[span_23](start_span)[span_23](end_span)

result = st.session_state.result[span_24](start_span)[span_24](end_span)[span_25](start_span)[span_25](end_span)
left, right = st.columns([1.7, 0.9], gap="large")[span_26](start_span)[span_26](end_span)[span_27](start_span)[span_27](end_span)

with left:[span_28](start_span)[span_28](end_span)[span_29](start_span)[span_29](end_span)
    if result["status"] == "grounded":[span_30](start_span)[span_30](end_span)[span_31](start_span)[span_31](end_span)
        st.success("Grounded answer")[span_32](start_span)[span_32](end_span)[span_33](start_span)[span_33](end_span)
        st.subheader(st.session_state.question)[span_34](start_span)[span_34](end_span)[span_35](start_span)[span_35](end_span)
        st.write(result["answer"])[span_36](start_span)[span_36](end_span)[span_37](start_span)[span_37](end_span)
        st.markdown("#### Sources")[span_38](start_span)[span_38](end_span)[span_39](start_span)[span_39](end_span)
        for section in result["sections"]:[span_40](start_span)[span_40](end_span)[span_41](start_span)[span_41](end_span)
            st.markdown([span_42](start_span)[span_42](end_span)
                f'<div class="source"><span class="source-title">{section["source"]}</span><br>{section["text"]}</div>',[span_43](start_span)[span_43](end_span)
                unsafe_allow_html=True,[span_44](start_span)[span_44](end_span)
            )[span_45](start_span)[span_45](end_span)
    else:[span_46](start_span)[span_46](end_span)
        st.warning("Insufficient evidence")[span_47](start_span)[span_47](end_span)
        st.subheader(st.session_state.question)[span_48](start_span)[span_48](end_span)
        st.write(result["answer"])[span_49](start_span)[span_49](end_span)
        st.info("Please check with the Travel Operations team for guidance.")[span_50](start_span)[span_50](end_span)

with right:[span_51](start_span)[span_51](end_span)
    st.markdown("### Retrieval result")[span_52](start_span)[span_52](end_span)
    if result["status"] == "grounded":[span_53](start_span)[span_53](end_span)
        st.success(f"Grounding score: {result['sections'][0]['score']:.2f}")[span_54](start_span)[span_54](end_span)
        for section in result["sections"]:[span_55](start_span)[span_55](end_span)
            st.markdown(f"**{section['source']}**")[span_56](start_span)[span_56](end_span)
            st.caption(f"Similarity score: {section['score']:.2f}")[span_57](start_span)[span_57](end_span)
    else:[span_58](start_span)[span_58](end_span)
        st.warning("Grounding score: 0.00 | Answer withheld")[span_59](start_span)[span_59](end_span)
        st.caption("PolicyPal only answers from indexed synthetic documents.")[span_60](start_span)[span_60](end_span)

