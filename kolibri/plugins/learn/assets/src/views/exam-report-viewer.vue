<template>

  <exam-report
    :examAttempts="examAttempts"
    :exam="exam"
    :userName="userName"
    :userId="userId"
    :currentAttempt="currentAttempt"
    :currentInteractionHistory="currentInteractionHistory"
    :currentInteraction="currentInteraction"
    :selectedInteractionIndex="selectedInteractionIndex"
    :questionNumber="questionNumber"
    :exercise="exercise"
    :itemId="itemId"
    :completionTimestamp="completionTimestamp"
    :closed="closed"
    :backPageLink="backPageLink"
    :navigateToQuestion="navigateToQuestion"
    :navigateToQuestionAttempt="navigateToQuestionAttempt"
  />

</template>


<script>

  import examReport from 'kolibri.coreVue.components.examReport';
  import { ClassesPageNames } from '../constants';

  export default {
    name: 'learnExamReportViewer',
    components: {
      examReport,
    },
    computed: {
      backPageLink() {
        return {
          name: ClassesPageNames.CLASS_ASSIGNMENTS,
          params: {
            classId: this.classId,
          },
        };
      },
    },
    methods: {
      navigateToQuestion(questionNumber) {
        this.navigateTo(questionNumber, 0);
      },
      navigateToQuestionAttempt(interaction) {
        this.navigateTo(this.questionNumber, interaction);
      },
      navigateTo(question, interaction) {
        this.$router.push({
          name: ClassesPageNames.EXAM_REPORT_VIEWER,
          params: {
            classId: this.classId,
            questionInteraction: interaction,
            questionNumber: question,
            examId: this.exam.id,
          },
        });
      },
    },
    vuex: {
      getters: {
        classId: state => state.pageState.exam.collection,
        examAttempts: state => state.pageState.examAttempts,
        exam: state => state.pageState.exam,
        userName: state => state.pageState.user.full_name,
        userId: state => state.pageState.user.id,
        currentAttempt: state => state.pageState.currentAttempt,
        currentInteractionHistory: state => state.pageState.currentInteractionHistory,
        currentInteraction: state => state.pageState.currentInteraction,
        selectedInteractionIndex: state => state.pageState.interactionIndex,
        questionNumber: state => state.pageState.questionNumber,
        exercise: state => state.pageState.exercise,
        itemId: state => state.pageState.itemId,
        completionTimestamp: state => state.pageState.examLog.completion_timestamp,
        closed: state => state.pageState.examLog.closed,
      },
    },
  };

</script>


<style lang="stylus" scoped></style>
